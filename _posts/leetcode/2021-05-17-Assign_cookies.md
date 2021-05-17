---
layout: post
title: Leetcode 455. 分发饼干
date: 2021-05-17
tags: [leetcode,贪心]
---
# 题目
假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/assign-cookies

# 题解
题目是求能满足的最大孩子数，即能让尽可能对的孩子吃上饼干，显然让胃口小的孩子能吃上饼干就行，选择能满足其胃口的最小的饼干就行。如果胃口小的孩子吃了一个比较大的饼干，那胃口大一点的可能就没得吃了。
```
public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int start=0;
        int res=0;
        for (int i=0;i<g.length;i++){
            for (int j=start;j<s.length;j++){
                # 当前的饼干能满足他的胃口就吃，然后考虑下一个孩子
                if (g[i]<=s[j]){
                    res++;
                    start=j+1;
                    break;
                }
            }
        }
        return res;
    }
```