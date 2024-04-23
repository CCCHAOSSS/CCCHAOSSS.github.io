---
title: 0423Leetcode
date: 2024-04-23 14:21:26
categories:
    -leetcode
tags:
    -leetcode
---

## 每日一题
2024/04/23

[1052](https://leetcode.cn/problems/grumpy-bookstore-owner/description/)
> 有一个书店老板，他的书店开了 n 分钟。每分钟都有一些顾客进入这家商店。给定一个长度为 n 的整数数组 customers ，其中 customers[i] 是在第 i 分钟开始时进入商店的顾客数量，所有这些顾客在第 i 分钟结束后离开。

> 在某些时候，书店老板会生气。 如果书店老板在第 i 分钟生气，那么 grumpy[i] = 1，否则 grumpy[i] = 0。
> 当书店老板生气时，那一分钟的顾客就会不满意，若老板不生气则顾客是满意的。
> 
> 书店老板知道一个秘密技巧，能抑制自己的情绪，可以让自己连续 minutes 分钟不生气，但却只能使用一次.
> 
> 请你返回 这一天营业下来，最多有多少客户能够感到满意 。


使用滑动窗口，同时结合grumpy数组进行窗口内增量的计算。
* 如果出去元素grumpy[i] == 1，则增量要减少customers[i],否则不变（因为base已经计算过了）
* 同时进来元素grumpy[i] == 1，则质量要增加ustomers[i]，否则不变

```java
class Solution {
    public int maxSatisfied(int[] customers, int[] grumpy, int minutes) {
        int base = 0;
        for(int i=0; i<customers.length; ++i){
            if(grumpy[i] == 0){
                base += customers[i];
            }
        }
        
        int more = 0;
        for(int i=0; i<minutes; ++i){
            more += customers[i] * grumpy[i] ;
        }
        int maxx = more;
        for(int i=minutes; i<customers.length; ++i){
            more = more - customers[i-minutes]*grumpy[i-minutes] + customers[i]*grumpy[i];
            maxx = Math.max(more, maxx);
        }

        return base + maxx;
    }
}
```

自己的想法也是滑动窗口但是，没想到增量这一点，是直接用窗口内绝对值来算，然后就要进行各种判断。。非常麻烦！（最后测试用例也没通过QAQ）


## hot100之分割回文串

[131](https://leetcode.cn/problems/palindrome-partitioning/?envType=study-plan-v2&envId=top-100-liked)
> 给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是 
回文串。返回 s 所有可能的分割方案。

这道题一看就是回溯了！
然后……没写出来ε(┬┬＿┬┬)3


## hot100之括号生成

[22](https://leetcode.cn/problems/generate-parentheses/?envType=study-plan-v2&envId=top-100-liked)
>数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

这也是一道回溯，比较简单，直接在左右括号里选择，不用遍历了，直接选。
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        if (n == 0) {
            return new ArrayList<>();
        }
        List<String> res = new ArrayList<>();
        StringBuilder track = new StringBuilder();
        backtrack(n, n, track, res);
        return res;
    }

    void backtrack(int left, int right,
        StringBuilder track, List<String> res) {
            if (right < left)
                return;
            if (left < 0 || right < 0)
                return;
            if (left == 0 && right == 0) {
                res.add(track.toString());
                return;
        }
        // 尝试放一个左括号
        track.append('('); 
        backtrack(left - 1, right, track, res);
        track.deleteCharAt(track.length() - 1); 

        // 尝试放一个右括号
        track.append(')'); 
        backtrack(left, right - 1, track, res);
        track.deleteCharAt(track.length() - 1); 
    }
}
```