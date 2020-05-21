## 53 - II. 0～n-1中缺失的数字

- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。</p>

<p>&nbsp;</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> [0,1,3]
<strong>输出:</strong> 2
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [0,1,2,3,4,5,6,7,9]
<strong>输出:</strong> 8</pre>

<p>&nbsp;</p>

<p><strong>限制：</strong></p>

<p><code>1 &lt;= 数组长度 &lt;= 10000</code></p>


## 解法：

因为是有序数组，如果 `i == num[i]` 说明下标 `i` 之前不缺少数字。如果缺少数字，比如少 `5`，那么下标 `5` 处的值一定大于 `5`。因此本题可以等价于找第一个与下标不一致的元素。

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int lo = 0, hi = nums.size();
        while(lo < hi){
            int mid = lo + (hi - lo) / 2;

            if(nums[mid] == mid){
                lo = mid + 1;
            }else{
                hi = mid;
            }
        }
        
        return lo;
    }
};
```