---
title: 环路检测
qid: 02.08
tags: [链表]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/linked-list-cycle-lcci/](https://leetcode-cn.com/problems/linked-list-cycle-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定一个有环链表，实现一个算法返回环路的开头节点。<br>有环链表的定义：在链表中某个节点的next元素指向在它前面出现过的节点，则表明该链表存在环路。</p><br><p><strong>示例 1：</strong><pre><strong>输入：</strong>head = [3,2,0,-4], pos = 1<br><strong>输出：</strong>tail connects to node index 1<br><strong>解释：</strong>链表中有一个环，其尾部连接到第二个节点。</pre></p><br><p><strong>示例 2：</strong><pre><strong>输入：</strong>head = [1,2], pos = 0<br><strong>输出：</strong>tail connects to node index 0<br><strong>解释：</strong>链表中有一个环，其尾部连接到第一个节点。</pre></p><br><p><strong>示例 3：</strong><pre><strong>输入：</strong>head = [1], pos = -1<br><strong>输出：</strong>no cycle<br><strong>解释：</strong>链表中没有环。</pre></p><br><p><strong>进阶：</strong><br>你是否可以不用额外空间解决此题？</p>

## 解法：

在链表的环上找一个点，然后从该点断开，问题就变成了寻找两个链表的交点的问题了。

![](https://wangyu-name.oss-cn-hangzhou.aliyuncs.com/superbed/2020/05/21/5ec69d01c2a9a83be535248f.jpg)

上图中 b 部分链表变成了以 head 和 fast 开头的两个链表的公共部分。


```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while(fast && fast->next){
            fast = fast->next->next;
            slow = slow->next;
            if(slow == fast){
                break;
            }
        }
        if(!fast || !fast->next){
            return nullptr;
        }
        fast = fast->next;
        slow->next = nullptr;

        ListNode *a = fast, *b = head;
        while(a != b){
            a = (a == nullptr) ? head : a->next;
            b = (b == nullptr) ? fast : b->next;
        }
        slow->next = fast;
        return a;
    }
};
```

在 `fast == slow` 处，fast 一共走过了 `a+b+c+b` 个节点，slow 走过了 `a+b` 个节点，且 fast 走过的节点数是 slow 走过的二倍。因此 `a+c+c+b = 2 * (a+b)`，由此易得 `c == a`。

因此，当 fast 和 slow 相交后，只需要使用两个指针从 slow 和 head 处一齐向前走，当两个指针指向同一节点时，该节点就是环的入口了。

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while(fast && fast->next){
            fast = fast->next->next;
            slow = slow->next;

            if(slow == fast){
                ListNode *a = fast, *b = head;
                while(a != b){
                    a = a->next;
                    b = b->next;
                }
                return a;
            }
            
        }
        return nullptr;
    }
};
```