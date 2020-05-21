## 35. 复杂链表的复制

- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>请实现 <code>copyRandomList</code> 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 <code>next</code> 指针指向下一个节点，还有一个 <code>random</code> 指针指向链表中的任意节点或者 <code>null</code>。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<p><img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png"></p>

<pre><strong>输入：</strong>head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
<strong>输出：</strong>[[7,null],[13,0],[11,4],[10,2],[1,0]]
</pre>

<p><strong>示例 2：</strong></p>

<p><img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png"></p>

<pre><strong>输入：</strong>head = [[1,1],[2,1]]
<strong>输出：</strong>[[1,1],[2,1]]
</pre>

<p><strong>示例 3：</strong></p>

<p><strong><img alt="" src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png"></strong></p>

<pre><strong>输入：</strong>head = [[3,null],[3,0],[3,null]]
<strong>输出：</strong>[[3,null],[3,0],[3,null]]
</pre>

<p><strong>示例 4：</strong></p>

<pre><strong>输入：</strong>head = []
<strong>输出：</strong>[]
<strong>解释：</strong>给定的链表为空（空指针），因此返回 null。
</pre>

<p>&nbsp;</p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>-10000 &lt;= Node.val &lt;= 10000</code></li>
	<li><code>Node.random</code>&nbsp;为空（null）或指向链表中的节点。</li>
	<li>节点数目不超过 1000 。</li>
</ul>

<p>&nbsp;</p>

<p><strong>注意：</strong>本题与主站 138 题相同：<a href="https://leetcode-cn.com/problems/copy-list-with-random-pointer/">https://leetcode-cn.com/problems/copy-list-with-random-pointer/</a></p>

<p>&nbsp;</p>


## 解法一：哈希表

使用原节点的地址作为 `key` 用拷贝的节点作为 `value`。先把所有节点拷贝一份，然后再把拷贝的节点串起来。


```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        unordered_map<Node*, Node*> mp;
		for(Node* p=head;p != nullptr;p = p->next){
			mp[p] = new Node(p->val);
		}
		for(Node* p=head;p != nullptr;p = p->next){
			mp[p].next = mp[p->next];
			mp[p].random = mp[p->random];
		}
		return mp[head];
    }
};
```

## 解法二：

复制链表中的每个节点，把该节点插入到原节点的后面。

```
1-->2-->3

1-->1-->2-->2-->3-->3
```

复制完了之后，修改指针的指向 `next == next->next`，`random = random->next`。

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head) return nullptr;
        
		Node *p = head;
		while(p != nullptr){
			Node *node = new Node(p->val);
			node->next = p->next;
			node->random = p->random;
			p->next = node;
			p = node->next;
		}
		p = head;
		while(p != nullptr){
            if(p->next->random){
    			p->next->random = p->next->random->next;
            }
			p = p->next->next;
		}

		p = head;
		head = p->next;
		while(p != nullptr){
			Node *copy = p->next;
			p->next = copy->next;
			p = p->next;
			if(copy->next){
				copy->next = p->next;
			}
		}

		return head;
    }
};
```


这个方法真巧秒啊。