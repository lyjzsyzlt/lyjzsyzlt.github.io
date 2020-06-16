---
layout: post
title: Attention机制总结
date: 2020-06-16
tags: ASR    
---

参考资料：<br>
1.[Attention Model（mechanism） 的 套路](https://blog.csdn.net/BVL10101111/article/details/78470716)

2.[A Brief Overview of Attention Mechanism](https://medium.com/syncedreview/a-brief-overview-of-attention-mechanism-13c578ba9129)

3.[深度学习中的注意力机制](https://blog.csdn.net/qq_40027052/article/details/78421155)

4.[Attention? Attention!](https://lilianweng.github.io/lil-log/2018/06/24/attention-attention.html)

5.[Effective Approaches to Attention-based Neural Machine Translation](https://arxiv.org/pdf/1508.04025.pdf)

其中[深度学习中的注意力机制](https://blog.csdn.net/qq_40027052/article/details/78421155)介绍的比较详细，可以仔细阅读

# 为什么使用Attention机制？
在我们看某张图片的某一时刻$t$，图片上只有一小块区域能够看得很仔细，其他部分会自动模糊处理，这样每一时刻会有更多的精力去处理一小块区域，效率更高。深度学习中的注意力机制与人的视觉注意力机制类似，即从众多的信息中选择对当前任务最关键的信息，因为不是所有的信息都是关键的。
# Attention机制的本质
**本质就是加权求和!!** <br>

在seq2seq模型中，encoder负责将原始输入特征进行高维抽象，比如将40维的fbank最终抽象成1024维的向量。decoder每一步的计算都需要encoder的输出，但是在decoder每一个时间步中，每一帧的语音所发挥的作用是不同的，所以需要对这些向量赋予不同的权重，然后使用这个权重对encoder各个状态进行加权求和得到context vector，最终将这个context vector输入到decoder进行计算。<br>
总结一下就是如下过程：<br>
记encoder的状态为$h_s$，decoder的状态为$h_t$ <br>

$$\begin{aligned}
   \alpha_{ts}&=\frac{exp(score(h_t,h_s))}{\sum_{s'=1}^Sexp(score(h_t,h_{s'}))}&计算权重(1) \\
c_t&=\sum_{s}\alpha_{ts}h_s&加权求和(2) \\
a_t&=f(c_t,h_t)=tanh(W_c[c_t;h_t])&输入decoder计算(3) 
\end{aligned}$$

根据不同的权重计算方式，得到不同的attention
1. Location-based Attention <br>
这个权重的计算只与decoder的状态有关，即$\alpha_t=softmax(W_ah_t)$
2. Concatenation(Additive)-based Attention <br>
   当前decoder的状态$h_t$分别与每个encoder的状态$h_s$进行concat操作，得到的结果作为该$h_s$的分数，即$score(h_t,h_s)=v_a^Ttanh(W_a[h_t;h_s])$，求出每一个score之后进行softmax归一化就是每个encoder状态的权重$\alpha$
3. dot-based attention <br>
   将encoder的状态和decoder的状态进行点乘，得到的结果作为该状态的分数，即$score(h_t,h_s)=h_t^Th_s$

self-attention 指的是序列（source或target）内部进行attention计算，这样可以学习到句子内部的一些句法信息，transformer模型完全基于self-attention，在语音识别，机器翻译等领域取得很不错的结果

![attention](https://upload-images.jianshu.io/upload_images/4434395-5125fc0176bb99bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)