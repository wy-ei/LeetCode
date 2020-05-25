## 26. 树的子结构

- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)</p>

<p>B是A的子结构， 即 A中有出现和B相同的结构和节点值。</p>

<p>例如:<br>
给定的树 A:</p>

<p><code>&nbsp; &nbsp; &nbsp;3<br>
&nbsp; &nbsp; / \<br>
&nbsp; &nbsp;4 &nbsp; 5<br>
&nbsp; / \<br>
&nbsp;1 &nbsp; 2</code><br>
给定的树 B：</p>

<p><code>&nbsp; &nbsp;4&nbsp;<br>
&nbsp; /<br>
&nbsp;1</code><br>
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>A = [1,2,3], B = [3,1]
<strong>输出：</strong>false
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>A = [3,4,5,1,2], B = [4,1]
<strong>输出：</strong>true</pre>

<p><strong>限制：</strong></p>

<p><code>0 &lt;= 节点个数 &lt;= 10000</code></p>


## 思路：

以二叉树 A 的每个节点为根，判断二叉树 B 是否为 A 中以该节点为根的二叉树的子结构。

这需要遍历二叉树 A，在每个节点上调用一个函数，判断是否为子结构。如果采用递归的方法遍历，就很麻烦，因为你不知道该在什么时候停止递归。采用层次遍历是比较好的做法。

```cpp
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if(B == nullptr) return false;
        if(A == nullptr) return false;
        return traversal(A, B);
    }

    bool traversal(TreeNode* root, TreeNode *B){
        queue<TreeNode*> node_queue;
        node_queue.push(root);
        while(!node_queue.empty()){
            TreeNode *node = node_queue.front();
            node_queue.pop();
            if(is_sub_structure(node, B)){
                return true;
            }
            if(node->left){
                node_queue.push(node->left);
            }
            if(node->right){
                node_queue.push(node->right);
            }
        }
        return false;
    }

    bool is_sub_structure(TreeNode* A_node, TreeNode* B_node){
        if(B_node == nullptr){
            return true;
        }
        if(A_node == nullptr){
            return false;
        }
        if(A_node->val != B_node->val){
            return false;
        }

        return is_sub_structure(A_node->left, B_node->left) &&
            is_sub_structure(A_node->right, B_node->right);
    }
};
```