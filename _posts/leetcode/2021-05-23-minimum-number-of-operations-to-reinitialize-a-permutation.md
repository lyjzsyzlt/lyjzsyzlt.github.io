---
layout: post
title: 1806. 还原排列的最少操作步数
date: 2021-05-19
tags: [leetcode,贪心]
---
# 题目
给你一个偶数 n​​​​​​ ，已知存在一个长度为 n 的排列 perm ，其中 perm[i] == i​（下标 从 0 开始 计数）。

一步操作中，你将创建一个新数组 arr ，对于每个 i ：

如果 i % 2 == 0 ，那么 arr[i] = perm[i / 2]
如果 i % 2 == 1 ，那么 arr[i] = perm[n / 2 + (i - 1) / 2]
然后将 arr​​ 赋值​​给 perm 。

要想使 perm 回到排列初始值，至少需要执行多少步操作？返回最小的 非零 操作步数。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-number-of-operations-to-reinitialize-a-permutation



# 题解
通过模拟我们可以发现，每次转移的路径是一样的，并且只要有一个数字回到了它原来的位置，那么整个序列就和之前的一致了。所以我们考虑2这个数字的转移路径，计算它再回到2这个位置的步数即可。当2i小于n的时候，通过公式可知，i下次应该往2i的位置转移，当超过n， i下次应该往2*i-n+1。如n=12的时候2的转移路径：4-8-5-10-9-7-3-6-1-2。
```
public int reinitializePermutation(int n) {
        if(n==2){
            return 1;
        }
        int i=2;
        int res=0;
        while(true){
            if(i*2<n){
                i=i*2;
            }else{
                i=2*i-n+1;
            }
            res++;
            if(i==2){
                break;
            }
        }
        return res;
    }
```