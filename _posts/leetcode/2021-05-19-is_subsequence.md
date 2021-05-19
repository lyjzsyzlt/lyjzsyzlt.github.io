---
layout: post
title: 392. 判断子序列
date: 2021-05-19
tags: [leetcode,贪心]
---
# 题目
给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/is-subsequence



# 题解
将t删除一些字符，或者不删，得到的结果s就是它的一个子串。
假设s[0]和t[i]匹配成功了，那么下一步我们会从t[i+1:]里面选择一个字符与s[1]匹配。即s[0:i]是t[0:j]的子串，那么我们只要考虑s[i+1:]是不是t[j+1:]的子串，如果是的，那么s就是t的子串。
```
ppublic boolean isSubsequence(String s, String t) {
        int l1=s.length();
        int l2=t.length();
        int i=0; // s的下标
        int j=0; // t的下标
        while(i<l1 && j<l2){
            // 匹配成功，i,j都往后移
            if(s.charAt(i)==t.charAt(j)){
                i++;
                j++;
            }else{
                // j往后移来匹配s[i]
                j++;
            }
        }
        // 要是i走到头了，说明匹配成功了
        return i==l1;
    }
```