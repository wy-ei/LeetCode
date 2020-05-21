## 18. 删除链表的节点

- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。</p>

<p>返回删除后的链表的头节点。</p>

<p><strong>注意：</strong>此题对比原题有改动</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> head = [4,5,1,9], val = 5
<strong>输出:</strong> [4,1,9]
<strong>解释: </strong>给定你链表中值为&nbsp;5&nbsp;的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -&gt; 1 -&gt; 9.
</pre>

<p><strong>示例 2:</strong></p>

<pre><strong>输入:</strong> head = [4,5,1,9], val = 1
<strong>输出:</strong> [4,5,9]
<strong>解释: </strong>给定你链表中值为&nbsp;1&nbsp;的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -&gt; 5 -&gt; 9.
</pre>

<p>&nbsp;</p>

<p><strong>说明：</strong></p>

<ul>
	<li>题目保证链表中节点的值互不相同</li>
	<li>若使用 C 或 C++ 语言，你不需要 <code>free</code> 或 <code>delete</code> 被删除的节点</li>
</ul>


## 解法：


```c++
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
		ListNode pre(-1);
		pre.next = head;
		ListNode *p = &pre;

		while(p->next != nullptr && p->next->val != val){
			p = p->next;
		}
		if(p->next != nullptr && p->next->val == val){
			ListNode *next = p->next;
			p->next = p->next->next;
			// free(next) or delete next;
		}
		return pre.next;
    }
};
```