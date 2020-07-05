---
title: 排序矩阵查找
qid: 10.09
tags: [双指针,二分查找,分治算法]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/sorted-matrix-search-lcci/](https://leetcode-cn.com/problems/sorted-matrix-search-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定M&times;N矩阵，每一行、每一列都按升序排列，请编写代码找出某元素。</p>

<p><strong>示例:</strong></p>

<p>现有矩阵 matrix 如下：</p>

<pre>[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
</pre>

<p>给定 target&nbsp;=&nbsp;<code>5</code>，返回&nbsp;<code>true</code>。</p>

<p>给定&nbsp;target&nbsp;=&nbsp;<code>20</code>，返回&nbsp;<code>false</code>。</p>


## 解法：

详细说明请参考 [剑指 Offer - 04. 二维数组中的查找](../lcof/04-er-wei-shu-zu-zhong-de-cha-zhao-lcof.md)

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix.back().empty()) return false;
        int row_lo = 0, row_hi = matrix.size();
        int col_lo = 0, col_hi = matrix.back().size();

        while(row_lo < row_hi && col_lo < col_hi){
            if(matrix[row_hi-1][col_lo] == target || matrix[row_lo][col_hi-1] == target){
                return true;
            }

            col_hi = upper_bound(matrix[row_lo].begin(), matrix[row_lo].end(), target) - matrix[row_lo].begin();
            row_hi = upper_bound(matrix.begin(), matrix.end(), target, [col_lo](int t, auto& row){
                return t < row[col_lo];
            }) - matrix.begin();


            if(row_hi > 0){
                col_lo = lower_bound(matrix[row_hi-1].begin(), matrix[row_hi-1].end(), target) - matrix[row_hi-1].begin();
            }
            
            if(col_hi > 0){
                row_lo = lower_bound(matrix.begin(), matrix.end(), target, [col_hi](auto& row, int t){
                    return row[col_hi-1] < t;
                }) - matrix.begin();
            }
            
        }
        return false;
    }
};
```

## 解法二：

找到矩阵中心位置的值 n，如果 `n < target`，那么 1 区域可以排除。如果 `n > target` 那么 4 区域可以排除，只需要在余下的区域寻找即可。

```
+------+------+
|      |      |
|   1  |  2   |
+----- n -----+
|      |      |
|   3  |  4   |
+------+------+
```

这个方法每次排除 1/4 的规模，设 r_i 为第 i 次 search 时没有被排除的区域占比

```
r_0 = 1
r_1 = 3/4 * r_0
r_2 = 3/4 * r_1
...
r_n = 3/4 * r_(n-1)
```

这里 n 等于 `log(min(M, N))`

因此需要查找的区域为 `(3/4)^n * M*N`，当 `min(M, N) = 50000` 时，`(3/4)^n` 约为 0.05，因此我感觉它是一个 O(M*N) 的算法，但是常数项比较小。

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix.back().empty()) return false;
        int row_lo = 0, row_hi = matrix.size();
        int col_lo = 0, col_hi = matrix.back().size();
        return serach(matrix, row_lo, row_hi, col_lo, col_hi, target);
    }

    bool serach(vector<vector<int>>&matrix, int row_lo, int row_hi, int col_lo, int col_hi, int target){
        if(row_lo == row_hi ||col_lo == col_hi) return false;
        int col_mid = col_lo + (col_hi - col_lo) / 2;
        int row_mid = row_lo + (row_hi - row_lo) / 2;
        int n = matrix[row_mid][col_mid];
        if(n == target){
            return true;
        }
        if(n > target){
            return serach(matrix, row_lo, row_hi, col_lo, col_mid, target) ||
                serach(matrix, row_lo, row_mid, col_mid, col_hi, target);
        }else{
            return serach(matrix, row_lo, row_hi, col_mid+1, col_hi, target) ||
                serach(matrix, row_mid+1, row_hi, col_lo, col_mid+1, target);
        }
    }
};
```