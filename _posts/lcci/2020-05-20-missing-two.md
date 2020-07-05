---
title: 消失的两个数字
qid: 17.19
tags: [数组,数学]
---


- 难度：Hard
- 题目链接：[https://leetcode-cn.com/problems/missing-two-lcci/](https://leetcode-cn.com/problems/missing-two-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定一个数组，包含从 1 到 N 所有的整数，但其中缺了两个数字。你能在 O(N) 时间内只用 O(1) 的空间找到它们吗？</p>

<p>以任意顺序返回这两个数字均可。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> <code>[1]</code>
<strong>输出: </strong>[2,3]</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> <code>[2,3]</code>
<strong>输出: </strong>[1,4]</pre>

<p><strong>提示：</strong></p>

<ul>
	<li><code>nums.length &lt;=&nbsp;30000</code></li>
</ul>


## 解法：

思路和 [56 - I. 数组中数字出现的次数](./lcof/56%20-%20I-shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof.md) 相同。

```c++
class Solution {
public:
    vector<int> missingTwo(vector<int>& nums) {
        int len = nums.size();
        int x = (len + 1) ^ (len + 2);
        for(int i=0;i<len;i++){
            x ^= (i+1) ^ nums[i];
        }

        int mask = x & ~(x-1);
        
        int a = 0, b = 0;
        for(int i=0;i<len;i++){
            if(nums[i] & mask){
                a ^= nums[i];
            }else{
                b ^= nums[i];
            }
        }
        for(int i=1;i<=len+2;i++){
            if(i & mask){
                a ^= i;
            }else{
                b ^= i;
            }
        }

        return {a, b};
    }
};
```