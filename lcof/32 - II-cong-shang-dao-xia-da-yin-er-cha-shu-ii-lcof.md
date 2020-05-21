## 32 - II. 从上到下打印二叉树 II

- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。</p>

<p>&nbsp;</p>

<p>例如:<br>
给定二叉树:&nbsp;<code>[3,9,20,null,null,15,7]</code>,</p>

<pre>    3
   / \
  9  20
    /  \
   15   7
</pre>

<p>返回其层次遍历结果：</p>

<pre>[
  [3],
  [9,20],
  [15,7]
]
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ol>
	<li><code>节点总数 &lt;= 1000</code></li>
</ol>

<p>注意：本题与主站 102 题相同：<a href="https://leetcode-cn.com/problems/binary-tree-level-order-traversal/">https://leetcode-cn.com/problems/binary-tree-level-order-traversal/</a></p>


## 解法：

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        if(root == nullptr) return ret;
        
        queue<TreeNode*> node_queue;
        node_queue.push(root);

        while(!node_queue.empty()){
            vector<int> vec;
            int n = node_queue.size();

            for(int i=0;i<n;i++){
                TreeNode *node = node_queue.front();
                node_queue.pop();
                vec.push_back(node->val);
                if(node->left){
                    node_queue.push(node->left);
                }
                if(node->right){
                    node_queue.push(node->right);
                }
            }

            ret.emplace_back(::move(vec));
        }

        return ret;
    }
};
```