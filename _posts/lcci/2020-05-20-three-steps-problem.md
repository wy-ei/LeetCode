---
title: 三步问题
qid: 08.01
tags: [动态规划]
---


- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/three-steps-problem-lcci/](https://leetcode-cn.com/problems/three-steps-problem-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>三步问题。有个小孩正在上楼梯，楼梯有n阶台阶，小孩一次可以上1阶、2阶或3阶。实现一种方法，计算小孩有多少种上楼梯的方式。结果可能很大，你需要对结果模1000000007。</p>

<p> <strong>示例1:</strong></p>

<pre>
<strong> 输入</strong>：n = 3 
<strong> 输出</strong>：4
<strong> 说明</strong>: 有四种走法
</pre>

<p> <strong>示例2:</strong></p>

<pre>
<strong> 输入</strong>：n = 5
<strong> 输出</strong>：13
</pre>

<p> <strong>提示:</strong></p>

<ol>
<li>n范围在[1, 1000000]之间</li>
</ol>


## 解法：

和青蛙跳台阶差不多。

```c++
class Solution {
public:
    int waysToStep(int n) {
        if(n == 1) return 1;
        if(n == 2) return 2;
        if(n == 3) return 4;
        int a = 1, b = 2, c = 4;
        for(int i=4; i<=n; i++){
            int ways = ((a + b) % 1000000007 + c) % 1000000007;
            a = b;
            b = c;
            c = ways;
        }
        return c;
    }
};
```