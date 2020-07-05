---
title:  字母与数字
qid: 17.05
tags: [数组]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/find-longest-subarray-lcci/](https://leetcode-cn.com/problems/find-longest-subarray-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定一个放有字符和数字的数组，找到最长的子数组，且包含的字符和数字的个数相同。</p>

<p>返回该子数组，若存在多个最长子数组，返回左端点最小的。若不存在这样的数组，返回一个空数组。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入: </strong>[&quot;A&quot;,&quot;1&quot;,&quot;B&quot;,&quot;C&quot;,&quot;D&quot;,&quot;2&quot;,&quot;3&quot;,&quot;4&quot;,&quot;E&quot;,&quot;5&quot;,&quot;F&quot;,&quot;G&quot;,&quot;6&quot;,&quot;7&quot;,&quot;H&quot;,&quot;I&quot;,&quot;J&quot;,&quot;K&quot;,&quot;L&quot;,&quot;M&quot;]

<strong>输出: </strong>[&quot;A&quot;,&quot;1&quot;,&quot;B&quot;,&quot;C&quot;,&quot;D&quot;,&quot;2&quot;,&quot;3&quot;,&quot;4&quot;,&quot;E&quot;,&quot;5&quot;,&quot;F&quot;,&quot;G&quot;,&quot;6&quot;,&quot;7&quot;]
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入: </strong>[&quot;A&quot;,&quot;A&quot;]

<strong>输出: </strong>[]
</pre>

<p><strong>提示：</strong></p>

<ul>
	<li><code>array.length &lt;= 100000</code></li>
</ul>


## 解法：

从左至右计算前缀和，数字视为 1 字母视为 -1，保存前缀和与下标的映射关系。如果下标 i 处前缀和为 +3，找到之前出现前缀和为 +3 的下标 j，那么 j ~ i 之间的和为 0。这个区间内字母的数量和数字的数量相同。

```c++
class Solution {
public:
    vector<string> findLongestSubarray(vector<string>& array) {
        unordered_map<int, int> mp;
        int n = 0;
        int start = 0, max_len = 0;

        mp[0] = 0;
        for(int i=0;i<array.size();i++){
            if(isdigit(array[i][0])){
                n++;
            }else{
                n--;
            }

            if(mp.find(n) == mp.end()){
                mp[n] = i+1;
            }else{
                if(i - mp[n] + 1 > max_len){
                    start = mp[n];
                    max_len = i - mp[n] + 1;
                }
            }
        }
        return vector<string>(array.begin() + start, array.begin() + start + max_len);
    }
};
```