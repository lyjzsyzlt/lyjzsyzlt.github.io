---
layout: post
title: 2020-06-22-pytorch tensor基本操作
date: 2020-06-22
tags: pytorch    
---

# torch.gather
使用场景：已知一个二维tensor $A_{m\times n}$，现在有个长度$m$的向量$b$，向量每个元素表示$A$对应行向量的index。比如：

![A](https://upload-images.jianshu.io/upload_images/4434395-0dfc626129bb7ba4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)<br>
$b$：[2,1,3]
意思就是，$A$第一行index=2的元素，$A$第二行index=1的元素，$A$第三行index=3的元素。即最终我们需要获得结果是$[0.4,0.3,0.2]$。我们可以使用gather方法一次性获取。

官方文档：https://pytorch.org/docs/master/generated/torch.gather.html
```
import torch

A = torch.tensor([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
index = torch.tensor([2, 0, 1])
# index的维度需要和A一致
index = index.view(-1, 1)
# dim=1表示在第一维上取
res = torch.gather(A, dim=1, index=index)
print('A:',A)
print('res1:', res)

index = torch.tensor([[2, 1], [0, 1], [1, 2]])
res = torch.gather(A, dim=1, index=index)
print('res2:', res)
```
![torch.gather](https://upload-images.jianshu.io/upload_images/4434395-98a00741ee420672.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

