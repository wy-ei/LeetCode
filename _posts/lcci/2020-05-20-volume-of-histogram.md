---
title: 直方图的水量
qid: 17.21
tags: [栈,数组,双指针]
---


- 难度：Hard
- 题目链接：[https://leetcode-cn.com/problems/volume-of-histogram-lcci/](https://leetcode-cn.com/problems/volume-of-histogram-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定一个直方图(也称柱状图)，假设有人从上面源源不断地倒水，最后直方图能存多少水量?直方图的宽度为 1。</p>

<p><img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png" style="height: 161px; width: 412px;"></p>

<p><small>上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的直方图，在这种情况下，可以接 6 个单位的水（蓝色部分表示水）。&nbsp;<strong>感谢 Marcos</strong> 贡献此图。</small></p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> [0,1,0,2,1,0,1,3,2,1,2,1]
<strong>输出:</strong> 6</pre>


## 解法一：

在每个位置上，只要知道该位置的左右两边的最低柱子的高度，就可以算当前柱子上的水量了。因此正向和反向遍历数组，计算出每个位置上左边和右边的最大高度。如此，根据当前位置的高度，就可以知道当前位置可以存储多少水了。

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        vector<int> max_height_left(height.size(), 0);
        vector<int> max_height_right(height.size(), 0);

        int max_height = 0;
        for(int i=0;i<height.size();i++){
            max_height_left[i] = max_height;
            max_height = max(max_height, height[i]);
        }
        
        max_height = 0;
        for(int i=height.size()-1; i>=0; i--){
            max_height_right[i] = max_height;
            max_height = max(max_height, height[i]);
        }

        int volume = 0;
        for(int i=0;i<height.size();i++){
            int min_height = min(max_height_left[i], max_height_right[i]);
            if(min_height > height[i]){
                volume += min_height - height[i];
            }
        }

        return volume;
    }
};
```

## 解法二：双指针

上面那种方法，需要存储下左右两边的最大高度，那其实是不必要的。一个柱子上的水量只依赖于本身高度和左右两边的最大高度中的较小者。考虑从左右两端起第二个柱子，它们分别知晓左边和右边的最大高度。这两个高度一比较，必然有一端更矮，矮的那一端就可以计算水量了。

计算完成后，可以更新最大高度，而后边上的两个柱子，其中一个又可以计算水量了。如此，就可以从两边开始累积水量，不需要额外的存储。

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        if(height.size() < 3) return 0;
        int l = 0, r = height.size()-1;
        int volume = 0;
        int l_max = height[l];
        int r_max = height[r];

        while(l < r){
            if(l_max < r_max){
                volume += max(0, l_max - height[l]);
                l++;
                l_max = max(l_max, height[l]);
            }else{
                volume += max(0, r_max - height[r]);
                r--;
                r_max = max(r_max, height[r]);
            }
        }
        return volume;
    }
};
```