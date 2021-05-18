---
layout: post
title: 122. 买卖股票的最佳时机 II
date: 2021-05-18
tags: [leetcode,贪心,动态规划]
---
# 题目
给定一个数组 prices ，其中 prices[i] 是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii


# 题解
题目说了**可以尽可能地完成更多的交易（多次买卖一支股票）**，所以当股票涨了我们就可以卖掉，然后当天再买入，为了后面涨了再卖。
```
public int maxProfit(int[] prices) {
        int n=prices.length;
        int res=0;
        for(int i=1;i<n;i++){
            res+=Math.max(0, prices[i]-prices[i-1]);
        }
        return res;
    }
```