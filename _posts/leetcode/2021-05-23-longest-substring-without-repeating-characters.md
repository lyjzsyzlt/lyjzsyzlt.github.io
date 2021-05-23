---
layout: post
title: 3. 无重复字符的最长子串
date: 2021-05-19
tags: [leetcode,滑动窗口]
---
# 题目
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。



# 题解
参考：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/hua-jie-suan-fa-3-wu-zhong-fu-zi-fu-de-zui-chang-z/

每往后遍历一个字符，先从map中查找是不是存在这个字符，存在的话，比较它对应的位置与start的大小，如果比start小，就不用管了，如果大于等于start，说明目前搜索到的序列包含当前字符，就需要将start移动到该位置的下一个位置，计算一次当前子串的长度。最后就将这个字符对应位置写入map中，方便下一次查找。
```
public int lengthOfLongestSubstring(String s) {
        if(s.equals("")){
            return 0;
        }
        int n=s.length();
        int res=0;
        Map<Character, Integer> map=new HashMap();
        char c;
        int start=0;
        for(int i=0;i<n;i++){
            c=s.charAt(i);
            if(map.containsKey(c)){
                // start=Math.max(map.get(c), start);
                if(map.get(c)>=start){
                    start=map.get(c)+1;
                }
            }
            res=Math.max(i-start+1, res);
            map.put(c,i); // map.put(c,i+1);
        }
        return res;
    }
```