---
title: 阶乘尾数
qid: 16.05
tags: [数学]
---


- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/factorial-zeros-lcci/](https://leetcode-cn.com/problems/factorial-zeros-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>设计一个算法，算出 n 阶乘有多少个尾随零。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> 3
<strong>输出:</strong> 0
<strong>解释:</strong>&nbsp;3! = 6, 尾数中没有零。</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> 5
<strong>输出:</strong> 1
<strong>解释:</strong>&nbsp;5! = 120, 尾数中有 1 个零.</pre>

<p><strong>说明: </strong>你算法的时间复杂度应为&nbsp;<em>O</em>(log&nbsp;<em>n</em>)<em>&nbsp;</em>。</p>


## 解法：

5 10 15 25 ... 这样的数乘以 2 的倍数就会产生 10 的倍数。另外，能被 5 整数的数远比能被 2 整除的数要少。因此一个数中含有因子 5，此数就能与某个偶数相乘得到一个 10，阶乘的结果中就会产生一个 0。另外 25 中包含两个 5， `25 * 2 * 4 = 200`。

每隔 5 个数，就有一个含因子 5 的，每隔 25 个数，就有一个含两个因子 5 的。因此，`n/5 + n/25 + n/125 + ...` 就是答案。

```c++
class Solution {
public:
    int trailingZeroes(int n) {
        int num = 0;
        while(n >= 5){
            n = n / 5;
            num += n;
        }
        return num;
    }
};
```