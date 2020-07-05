---
title: 求和路径
qid: 04.12
tags: [树,深度优先搜索]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/paths-with-sum-lcci/](https://leetcode-cn.com/problems/paths-with-sum-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定一棵二叉树，其中每个节点都含有一个整数数值(该值或正或负)。设计一个算法，打印节点数值总和等于某个给定值的所有路径的数量。注意，路径不一定非得从二叉树的根节点或叶节点开始或结束，但是其方向必须向下(只能从父节点指向子节点方向)。</p>

<p><strong>示例:</strong><br>
给定如下二叉树，以及目标和&nbsp;<code>sum = 22</code>，</p>

<pre>              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
</pre>

<p>返回:</p>

<pre>3
<strong>解释：</strong>和为 22&nbsp;的路径有：[5,4,11,2], [5,8,4,5], [4,11,7]</pre>

<p>提示：</p>

<ul>
	<li><code>节点总数 &lt;= 10000</code></li>
</ul>


## 解法一：递归

从每个节点开始向下求和。路径求和部分，很好操作一个递归的遍历即可。问题是如何在每个节点上再次做遍历。一种方法是写两个遍历函数，其中一个函数在遍历的过程中，在每个节点上调用另一个函数，计算以该节点为起点的符合要求的路径个数。

但其实 `pathSum` 这个函数本身就可以进行递归调用，因此只需要在写一个遍历函数，从指定节点出发进行求和，返回符合要求的路径个数。

```c++
class Solution {
public:
    int pathSum(TreeNode* root, int sum) {
        if(root == 0){
            return 0;
        }
        int num = traversal(root, sum);
        num += pathSum(root->left, sum);
        num += pathSum(root->right, sum);
        return num;
    }

private:
    int traversal(TreeNode* node, int target){
        if(!node) return 0;
        int num = 0;
        target -= node->val;

        if(0 == target){
            num += 1;
        }
        num += traversal(node->left, target);
        num += traversal(node->right, target);
        return num;
    }
};
```

## 解法二：前缀和

在一个序列中，一个值及其之前的所有值的和，叫做前缀和。达某个节点的路径为一个序列，该序列上的每个节点，都有一个前缀和。如果某个靠后的节点的前缀和减去前面的一个前缀和的值为 n，那么说明两个节点间的所有节点的和为 n。

基于此想法，在深度遍历的时候，记录下每个节点的前缀和，构成一个表，key 为前缀和，value 为有此前缀和的数量。然后用当前节点的前缀和减去 sum 得到 n，用 n 去表中查询。如果能够找到，说明必然以之前的某个点为起点，当前点为终点构成的路径之和为 sum。

```c++
class Solution {
public:
    int pathSum(TreeNode* root, int sum) {
        unordered_map<int, int> prefix_sum_to_count_map;
        prefix_sum_to_count_map[0] = 1;

        int num = traversal(root, prefix_sum_to_count_map, 0, sum);
        return num;
    }

private:
    int traversal(TreeNode* node, unordered_map<int, int>& mp, int sum, int target){
        if(!node) return 0;
        sum += node->val;
        int num = 0;
        num += mp[sum-target]; // return 0 if not exists
        mp[sum] += 1;
        num += traversal(node->left, mp, sum, target);
        num += traversal(node->right, mp, sum, target);
        mp[sum] -= 1;
        return num;
    }
};
```