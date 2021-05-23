---
layout: post
title: 1685. 有序数组中差绝对值之和
date: 2021-05-23
tags: [leetcode,前缀和]
---
# 题目
给你一个 非递减 有序整数数组 nums 。

请你建立并返回一个整数数组 result，它跟 nums 长度相同，且result[i] 等于 nums[i] 与数组中所有其他元素差的绝对值之和。

换句话说， result[i] 等于 sum(|nums[i]-nums[j]|) ，其中 0 <= j < nums.length 且 j != i （下标从 0 开始）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sum-of-absolute-differences-in-a-sorted-array


# 题解
定义前缀和prefix[i]=a[0]+...+a[i]，则有prefix[i]=prefix[i-1]+a[i], prefix[0]=a[0]

记返回的结果为res[0..n-1]，则

res[i]=(a[i]-a[0])+(a[i]-a[1])+(a[i]-a[2])+...+(a[i]-a[i-1])+(a[i]-a[i])+(a[n-1]-a[i])+(a[n-2]-a[i])+...+(a[i+1]-a[i])

整理可得：
res[i]=i\*a[i]-(prefix[i]-a[i])+prefix[n-1]-prefix[i]-a[i]\*(n-i-1)
=(2\*i-n+2)\*a[i]-2*prefix[i]+prefix[n-1];
```
public int[] getSumAbsoluteDifferences(int[] nums) {
        int n=nums.length;
        int[] prefix=new int[n];
        int[] res=new int[n];

        prefix[0]=nums[0];
        for(int i=1;i<n;i++){
            prefix[i]=prefix[i-1]+nums[i];
        }
        for(int i=0;i<n;i++){
            // res[i]=i*nums[i]-(prefix[i]-nums[i]) + prefix[n-1]-prefix[i]-(n-i-1)*nums[i];
            res[i]=(2*i-n+2)*nums[i]-2*prefix[i]+prefix[n-1];
        }
        return res;
    }
```