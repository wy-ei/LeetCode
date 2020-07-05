---
title: 搜索旋转数组
qid: 10.03
tags: [数组,二分查找]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/search-rotate-array-lcci/](https://leetcode-cn.com/problems/search-rotate-array-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>搜索旋转数组。给定一个排序后的数组，包含n个整数，但这个数组已被旋转过很多次了，次数不详。请编写代码找出数组中的某个元素，假设数组元素原先是按升序排列的。若有多个相同元素，返回索引值最小的一个。</p>

<p><strong>示例1:</strong></p>

<pre><strong> 输入</strong>: arr = [15, 16, 19, 20, 25, 1, 3, 4, 5, 7, 10, 14], target = 5
<strong> 输出</strong>: 8（元素5在该数组中的索引）
</pre>

<p><strong>示例2:</strong></p>

<pre><strong> 输入</strong>：arr = [15, 16, 19, 20, 25, 1, 3, 4, 5, 7, 10, 14], target = 11
<strong> 输出</strong>：-1 （没有找到）
</pre>

<p><strong>提示:</strong></p>

<ol>
	<li>arr 长度范围在[1, 1000000]之间</li>
</ol>


## 解法：

先找到旋转点，然后进行二分查找。

```c++
class Solution {
public:
    int search(vector<int>& arr, int target) {
        int lowest_index = find_lowest(arr);

        if(target < arr[lowest_index]){
            return -1;
        }
        if(lowest_index > 0 && target > arr[lowest_index-1]){
            return -1;
        }

        int lo, hi;
        if(target >= arr[0]){
            hi = lowest_index;
            lo = 0;
        }else{
            lo = lowest_index;
            hi = arr.size();
        }

        int idx = lower_bound(arr, lo, hi, target);

        return arr[idx] == target ? idx : -1;
    }

    int find_lowest(vector<int>& arr){
        int lo = 0, hi = arr.size();
        while(1 < hi && arr[0] == arr[hi-1]){
            hi--;
        }
        while(lo < hi){
            int mid = lo + (hi - lo) / 2;
            if(arr[mid] >= arr[0]){
                lo = mid + 1;
            }else{
                hi = mid;
            }
        }
        return lo;
    }

    int lower_bound(vector<int>& arr, int lo, int hi, int target){
        while(lo < hi){
            int mid = lo + (hi - lo) / 2;
            if(arr[mid] < target){
                lo = mid + 1;
            }else{
                hi = mid;
            }
        }
        return lo;
    }
};
```