---
title: 连续数列
qid: 16.17
tags: [数组,分治算法,动态规划]
---


- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/contiguous-sequence-lcci/](https://leetcode-cn.com/problems/contiguous-sequence-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定一个整数数组，找出总和最大的连续数列，并返回总和。</p>

<p><strong>示例：</strong></p>

<pre><strong>输入：</strong> [-2,1,-3,4,-1,2,1,-5,4]
<strong>输出：</strong> 6
<strong>解释：</strong> 连续子数组 [4,-1,2,1] 的和最大，为 6。
</pre>

<p><strong>进阶：</strong></p>

<p>如果你已经实现复杂度为 O(<em>n</em>) 的解法，尝试使用更为精妙的分治法求解。</p>


## 解法：

当 `sum < 0` 的时候，构成 sum 的那些元素必然不会是最大连续序列的一部分。因此可以把 `sum` 归 0。

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max_sum = INT_MIN;
        int sum = 0;
        for(int i=0;i<nums.size();i++){
            sum += nums[i];
            max_sum = max(max_sum, sum);
            sum = max(0, sum);
        }
        return max_sum;
    }
};
```