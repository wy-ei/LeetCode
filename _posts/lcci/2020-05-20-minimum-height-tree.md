---
title: 最小高度树
qid: 04.02
tags: [树,深度优先搜索]
---


- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/minimum-height-tree-lcci/](https://leetcode-cn.com/problems/minimum-height-tree-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定一个有序整数数组，元素各不相同且按升序排列，编写一个算法，创建一棵高度最小的二叉搜索树。</p><strong>示例:</strong><pre>给定有序数组: [-10,-3,0,5,9],<br><br>一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：<br><br>          0 <br>         / &#92 <br>       -3   9 <br>       /   / <br>     -10  5 <br></pre>

## 解法：

因为是排序的数组，因此数组的中间位置的元素一定是二叉搜索树的根节点。数组左半部分构成左子树，右半部分构成右子树。如此递归地建树，就可以构建一棵叶子节点的高度差最大为 1 的二叉搜索树。满二叉树，高度自然最小。

```c++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedArrayToBST(nums, 0, nums.size());
    }

private:
    TreeNode* sortedArrayToBST(vector<int>& nums, int lo, int hi) {
        if(hi == lo) return nullptr;

        int mid = lo + (hi - lo) / 2;
        
        auto *root = new TreeNode(nums[mid]);
        root->left = sortedArrayToBST(nums, lo, mid);
        root->right = sortedArrayToBST(nums, mid+1, hi);

        return root;
    }
};
```