---
title: 按摩师
qid: 17.16
tags: [动态规划]
---


- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/the-masseuse-lcci/](https://leetcode-cn.com/problems/the-masseuse-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟数。</p>

<p><strong>注意：</strong>本题相对原题稍作改动</p>



<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong> [1,2,3,1]
<strong>输出：</strong> 4
<strong>解释：</strong> 选择 1 号预约和 3 号预约，总时长 = 1 + 3 = 4。
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong> [2,7,9,3,1]
<strong>输出：</strong> 12
<strong>解释：</strong> 选择 1 号预约、 3 号预约和 5 号预约，总时长 = 2 + 9 + 1 = 12。
</pre>

<p><strong>示例 3：</strong></p>

<pre><strong>输入：</strong> [2,1,4,5,3,1,1,3]
<strong>输出：</strong> 12
<strong>解释：</strong> 选择 1 号预约、 3 号预约、 5 号预约和 8 号预约，总时长 = 2 + 4 + 3 + 3 = 12。
</pre>


## 解法：

动态规划即可。`dp[i]` 表示接受 `nums[i]` 预约时，目前能够得到的最大时长。

```c++
class Solution {
public:
    int massage(vector<int>& nums) {
        int len = nums.size();
        if(len == 0){
            return 0;
        }
        if(len == 1 || len == 2){
            return *max_element(nums.begin(), nums.end());
        }
        vector<int> dp(len, 0);
        for(int i=0;i<len;i++){
            int n1 = 0, n2 = 0;
            if(i - 3 >= 0){
                n1 = dp[i-3];
            }
            if(i - 2 >= 0){
                n2 = dp[i-2];
            }
            dp[i] = max(n1, n2) + nums[i];
        }
        return max(dp[len-1], dp[len-2]);
    }
};
```