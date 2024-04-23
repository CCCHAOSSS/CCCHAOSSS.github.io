---
title: 0422Leetcode记录
date: 2024-04-22 09:23:33
categoties:
    - leetcode
tags:
    - leetcode
---

# 0422Leetcode记录

今天也是不想背八股的一天，刷题！

## 每日一题
[力扣377题组合总和IV](https://leetcode.cn/problems/combination-sum-iv/description/)

> 给你一个由 **不同** 整数组成的数组 `nums` ，和一个目标整数 `target` 。请你从 `nums` 中找出并返回总和为 `target` 的元素组合的个数。

首先是今天的每日一题，先是用回溯法写，测试用例通过但是提交超时。
<!--more-->
```java
class Solution {
    int res = 0;
    public int combinationSum4(int[] nums, int target) {
        Arrays.sort(nums);
        traback(nums, target, 0, 0);
        return res;
    }

    public void traback(int[] nums, int target, int sum, int index){
        if(sum == target){
            res += 1;
            return;
        }
        
        for(int i=0; i<nums.length; ++i){
            if(sum > target){
                break;
            }
            sum += nums[i];
            traback(nums, target, sum, i+1);
            sum -= nums[i];
        }
    }
}
```

最后实在想不出来看了题解，居然用动态规划？？

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for(int i=1; i<=target; ++i){
            for(int num : nums){
                if(num <= i){
                    dp[i] += dp[i-num];
                }
            }
        }
        return dp[target];
    }
}
```

## hot100单词搜索
```java
class Solution{
    public boolean exist(char[][] board, String word){
        int m = board.length;
        int n = board[0].length;
        boolean visited = new boolean[m][n];
        for(int i=0; i<m; ++i){
            for(int j=0; j<n; ++j){
                boolean tmp = check(board, visited, i, j, word, 0);
                if(tmp){
                    return true;
                }
            }
        }
        return false;
    }

     public boolean check(char[][] board, boolean[][] visited, int i, int j, String s, int k){
        if(board[i][j] != s.charAt(k)){
            return false;
        }else if(k == s.length()-1){
            return true;
        }

        visited[i][j] = true;
        int[][] dirs = { {1,0}, {-1,0}, {0,1}, {0,-1} };
        boolean res = false;
        for(int[] dir : dirs){
            int newi = i+dir[0], newj = j+dir[1];
            if(newi>=0&&newi<board.length && newj>=0&&newj<board[0].length){
                if(!visited[newi][newj]){
                    boolean tag = check(board, visited, newi, newj, s, k+1);
                    if(tag){
                        res = true;
                        break;
                    }
                }
                
            }
        }
        visited[i][j] = false;
        return res;
    }
}
```