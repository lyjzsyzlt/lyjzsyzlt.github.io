---
layout: post
title: 1717. 删除子字符串的最大得分
date: 2021-05-22
tags: [leetcode,贪心]
---
# 题目
给你一个字符串 s 和两个整数 x 和 y 。你可以执行下面两种操作任意次。
删除子字符串 "ab" 并得到 x 分。
比方说，从 "cabxbae" 删除 ab ，得到 "cxbae" 。
删除子字符串"ba" 并得到 y 分。
比方说，从 "cabxbae" 删除 ba ，得到 "cabxe" 。
请返回对 s 字符串执行上面操作若干次能得到的最大得分。


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-score-from-removing-substrings


# 题解
首先有两种操作，分数不一样，最终要求最大的分数，很可能就是贪心解法。（事实上就是贪心），根据贪心算法，应该是先把分数大的串删完，然后再去删除分数小的串，思想很简单，本题的关键点在于怎么去实现，比如ab代表的分数大，那么我们首先要删除所有的ab，每删一次ab字符串都会发生了变化，可能会产生新的ab，为了将ab删除尽，暴力方法就是每次从头遍历，直到删完，这样会很耗时。

首先为了减少代码量，我们认为ab操作对应的分数大，如果ab对应的分数小，那么交换x，y，并将s逆序，这样每次我们优先删除ab即可。从头开始遍历，遇到的字符不是b，那么压入栈s1；当是b的时候，那么我们去看s1的栈顶是不是a，是的话，匹配成功，把a出栈，分数+x。这样遍历完，所有的ab都会被删除了。接下来，我们去删除ba，按照刚才的思想，我们考虑s1的栈顶元素，如果不是b，那么入栈s2；是b的话，去看s2的栈顶是不是a，是的话，匹配成功，a出栈，分数+y。直到s1栈空结束。
```
int score=0;

    public int maximumGain(String s, int x, int y) {
        if(x<y){
            x=x^y;
            y=x^y;
            x=x^y;
//            System.out.println(x+" "+y);
            s=new StringBuilder(s).reverse().toString();
        }
        Stack<Character> s1=new Stack<>();
        Stack<Character> s2=new Stack<>();
        for(int i=0;i<s.length();i++){
            char t = s.charAt(i);
            if(t!='b'){
                s1.push(t);
            }else{
                if (!s1.isEmpty() && s1.peek()=='a'){
                    s1.pop();
                    score+=x;
                }else {
                    s1.push(t);
                }
            }
        }
//        System.out.println(s1.toString()+ " " +score);

        while (!s1.isEmpty()){
            char c = s1.pop();
//            System.out.println(c);
            if (c!='b'){
                s2.push(c);
            }else{
                if(!s2.isEmpty() && s2.peek()=='a'){
                    s2.pop();
                    score+=y;
                }else {
                    s2.push(c);
                }
            }
        }
        return score;
    }
```