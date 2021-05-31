---
layout: post
title: pytorch scatter的用法
date: 2021-05-15
tags: [pytorch]
---


官方给的用法：
```
scatter(dim, index, src)
self[index[i][j][k]][j][k] = src[i][j][k]  # if dim == 0
self[i][index[i][j][k]][k] = src[i][j][k]  # if dim == 1
self[i][j][index[i][j][k]] = src[i][j][k]  # if dim == 2
```

一个例子
```
import torch
input = torch.randn(2, 4)
print(input)
output = torch.zeros(2, 5)
index = torch.tensor([[3, 1, 2, 0], [1, 2, 0, 3]])
output = output.scatter(1, index, input)
print(output)
````
输出：
```
tensor([[ 0.0461,  0.4024, -1.0115,  0.2167],
        [-0.6123,  0.5036,  0.2310,  0.6931]])
tensor([[ 0.2167,  0.4024, -1.0115,  0.0461,  0.0000],
        [ 0.2310, -0.6123,  0.5036,  0.6931,  0.0000]])
```
scatter（scatter_）是将input tensor按照index赋值给output tensor来达到更新output的效果的。
我们从index下手，
index[0][0]=3，由于dim=1，那么我们取input[0][0]=0.0461， 赋值给output[0][index[0][0]]=output[0][3]
index[0][3]=0, input[0][3]=0.2167，赋值给output[0][inde[0][3]]=output[0][0]
index[1][2]=0, input[1][2]=0.2310， 赋值给output[1][index[1][2]]=output[1][0]
index[1][3]=3, input[1][3]=0.6931， 赋值给output[1][index[1][3]]=output[1][3]

也就是index的下标和input的下标是一致的，取出来的这个值赋值给谁呢，这个是index对应的值以及dim来确定的，如果dim=1, 那么更新的是output[i][index[i][j]]=input[i][j]，官方文档给的是三维的情况，dim是多少，那么index的值就放在第几维。
scatter一个很重要的应用就是生成one-hot矩阵
假设总共有5类，现在一个batch有3个样本，分别对应的标签为1,2,0。那么生成的one-hot矩阵应该是这样的：
```
index=torch.tensor([[1], [2], [0]])
y=torch.zeros(3, 5)
y=y.scatter(1, index, 1)
print(y)
```
输出：
```
tensor([[0., 1., 0., 0., 0.],
        [0., 0., 1., 0., 0.],
        [1., 0., 0., 0., 0.]])
```

