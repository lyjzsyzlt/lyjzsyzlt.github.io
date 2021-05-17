---
layout: post
title: CNN原理详解及pytorch实现
date: 2021-05-17
tags: pytorch    
---

参考资料：
1. [【DL笔记6】从此明白了卷积神经网络（CNN）](https://www.jianshu.com/p/c0215d26d20a)

# 为什么不用DNN？
- DNN参数量太大，训练困难，容易过拟合
- 图像，语音中存在固有的局部模式

# CNN的优势
- 参数共享，同一组卷积核在图像上移动
- 稀疏连接，只计算卷积核对应区域的结果，可以很好地捕获局部信息
- 平移等变性，将输入图像的某一部分移动到另外一部分，在输出图像上也会有着相应的移动

![CNN演示，来自 https://www.zhihu.com/question/52668301/answer/1231346589](/images/posts/2021-05-17-15-15-15.webp)


# CNN计算
记输入维度为$m \times n$，卷积核维度为$k1 \times k2$，paddding大小为$p$，步长为$s$，则输出维度为：
$$x = (m-k1+2*p)/s+1$$
$$y=(n-k2+2*p)/s+1$$

![多通道CNN演示，来源zhuanlan.zhihu.com/p/29119239](/images/posts/2021-05-17-15-16-30.jpg)

![详细计算过程，来自 zhuanlan.zhihu.com/p/251068800](/images/posts/2021-05-17-15-17-56.png)


# 为什么需要池化层？
1) 池化层可以减少feature map的尺寸, 进而减少计算量. 当然stride大于1的卷积层也可以减少feature map的尺寸.

2) 池化层可以增加感受野. 不过卷积核尺寸大于1的卷积层同样也可以增加感受野.

3) 池化层可以带来特征的平移, 旋转等不变性.

4) (最大值等)池化层一般是非线性操作, 可以增强网络的表达能力. 激活层一般也是非线性的.

来自：https://www.zhihu.com/question/401688068/answer/1295651905

# CNN前向传播实现
![](/images/posts/2021-05-17-15-19-56.png)

我的简单实现：

```
import numpy as np
import random
import os
import torch

def seed_torch(seed=1029):
    random.seed(seed)
    os.environ['PYTHONHASHSEED'] = str(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)  # if you are using multi-GPU.
    torch.backends.cudnn.benchmark = False
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.enabled = False


def img2col(img, kernel_size, stride=1):
    """
    将输入矩阵img按照滑动方式取出来，组成新的矩阵，方便与卷积的参数直接矩阵乘
    :param img: N x C x H x W
    :param kernel_size: K
    :param stride: S
    :return: N x (out_h x out_w) x (K x K x C)
    """
    N, C, H, W = img.shape
    K = kernel_size
    S = stride
    out_h = (H - K) // S + 1
    out_w = (W - K) // S + 1

    col = []
    for i in range(0, out_h):
        h_min = i * S
        h_max = h_min + K
        for j in range(0, out_w):
            w_min = j * S
            w_max = w_min + K
            col.append(img[:, :, h_min:h_max, w_min:w_max].reshape(N, 1, -1))
    col = torch.cat(col, dim=1)

    # print(img[0,:,0:K, 0:K])
    # print(col[0,0,:])  两者的结果应一致
    return col


class Conv():
    def __init__(self, in_channels, out_channels, kernel_size, stride=1):
        self.in_ch = in_channels
        self.out_ch = out_channels
        self.k = kernel_size
        self.s = stride
        self.weight = torch.rand(self.in_ch, self.out_ch, self.k, self.k)
        self.bias = torch.rand(self.out_ch)

    def __call__(self, x):
        return self.forward(x)

    def forward(self, x):
        """
        卷积的前向操作
        :param x: 输入 N x in_ch x H x W
        :return: N x out_ch x out_h x out_w
        """
        N = x.shape[0]
        H = x.shape[2]
        W = x.shape[3]

        out_h = (H - self.k) // self.s + 1
        out_w = (W - self.k) // self.s + 1

        col = img2col(x, self.k, self.s)  # N x (out_h x out_w) x (K x K x in_ch)

        weight = self.weight.view(self.out_ch, self.k * self.k * self.in_ch).transpose(0, 1)  # (in_ch x K x K) x out_ch
        res = (torch.matmul(col, weight) + self.bias.view(1, 1, -1)).transpose(1, 2).contiguous().reshape(N, self.out_ch, out_h,out_w)
        return res


class MaxPool():
    def __init__(self, kernel_size, stride=1):
        self.k = kernel_size
        self.s = stride

    def __call__(self, x):
        return self.forward(x)

    def forward(self, x):
        N = x.shape[0]
        in_ch = x.shape[1]
        out_h = (x.shape[2] - self.k) // self.s + 1  # 输入的高
        out_w = (x.shape[3] - self.k) // self.s + 1  # 输入的宽

        col = img2col(x, self.k, self.s).view(N, -1, self.k * self.k)
        res = torch.max(col, dim=-1)[0].view(N, -1, in_ch).transpose(1, 2).contiguous().view(N, in_ch, out_h, out_w)
        return res

if __name__ == '__main__':
    in_ch = 5  # 输入通道数
    out_ch = 3  # 输出通道数
    K = 4  # kernel大小
    S = 2  # stride大小
    N = 100  # batch
    H = W = 28  # 输入图片大小

    # 固定随机种子
    seed_torch(1234)

    # 使用pytorch计算的卷积结果
    model1 = torch.nn.Conv2d(in_ch, out_ch, K, S)
    x = torch.rand(N, in_ch, H, W)
    gt = model1(x)
    max_pool1 = torch.nn.MaxPool2d(2, 3)
    gt = max_pool1(gt)

    # 使用自己实现的卷积，weight和bias使用pytorch中的数值
    model2 = Conv(in_ch, out_ch, K, S)
    model2.weight = model1.weight
    model2.bias = model1.bias
    pre = model2(x)
    max_pool2 = MaxPool(2, 3)
    pre = max_pool2(pre)

    # 两者的结果应该是一致的
    print(gt[0, 0, 1:5, 2:8])
    print(pre[0, 0, 1:5, 2:8])
```
![输出结果](/images/posts/2021-05-17-15-20-21.png)
# CNN反向传播



