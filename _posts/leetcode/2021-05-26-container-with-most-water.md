---
layout: post
title: 11. 盛最多水的容器
date: 2021-05-26
tags: [leetcode,双指针]
---
# 题目
给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器。

示例 1：
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/container-with-most-water

# 题解
双指针left, right分别指向0和n-1的位置，然后考虑下一步移动left还是right，结论是移动left和right对应值较小的那个。
假设原来left和right指向的高度分别为x,y，并且$x<y$，那么面积$S_0=min(x,y)*(right-left)=x*(right-left)$。假设我们下一步移动的是right（对应较大的高度那个），移动后的高度为$y'$，那么$S_1=min(x,y')*(right-left-1)\le x*(right-left-1)\lt S_0$。所以移动较大的那个必然会使得面积变小。所以我们只有让left右移，才能得到最大的面积。
```
public int maxArea(int[] height) {
        int n=height.length;
        int res=0;
        int l=0;
        int r=n-1;
        while(l<r){
            int s=Math.min(height[l],height[r])*(r-l);
            res=Math.max(s, res);
            if(height[l]<height[r]){
                l++;
            }else{
                r--;
            }
        }
        return res;
    }
```