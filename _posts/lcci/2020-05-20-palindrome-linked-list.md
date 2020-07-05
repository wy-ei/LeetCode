---
title: 回文链表
qid: 02.06
tags: [链表]
---


- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/palindrome-linked-list-lcci/](https://leetcode-cn.com/problems/palindrome-linked-list-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>编写一个函数，检查输入的链表是否是回文的。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入： </strong>1-&gt;2
<strong>输出：</strong> false 
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入： </strong>1-&gt;2-&gt;2-&gt;1
<strong>输出：</strong> true 
</pre>

<p>&nbsp;</p>

<p><strong>进阶：</strong><br>
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？</p>


## 解法一：拆分，翻转，比较，恢复

找到链表的中点，然后翻转后一部分，然后比较两个链表是否相同。由于原链表长度可能是奇数，因此在比较的时候消耗完最短的那个链表即可。

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == NULL){
            return true;
        }
        
        ListNode *fast, *slow;
        fast = slow = head;
        while(fast->next && fast->next->next){
            fast = fast->next->next;
            slow = slow->next;
        }
        ListNode *head2 = reverse_list(slow->next);
        bool ret = is_list_equal(head, head2);
        slow->next = reverse_list(head2);
        return ret;
    }

    static ListNode* reverse_list(ListNode *head){
        ListNode *p = NULL;
        while(head){
            ListNode *next = head->next;
            head->next = p;
            p = head;
            head = next;
        }
        return p;
    }
    
    static bool is_list_equal(ListNode *l1, ListNode *l2) {
        while (l1 && l2) {
            if (l1->val != l2->val) {
                return false;
            }
            l1 = l1->next;
            l2 = l2->next;
        }
        return true;
    }
};
```

## 解法二：递归

此解法在空间复杂度上不满足要求。

传入两个节点，开始时都指向头结点。第一个节点在递归进入的时候指向 next，第二个节点在递归退出的时候设置为 next，且第二个指针使用引用类型。如此，在每层递归中，这两个节点就是关于链表中心对称的两个节点。

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == NULL){
            return true;
        }
        ListNode* &tail = head;
        return isPalindrome(head, tail);
    }
private:
    bool isPalindrome(ListNode* head, ListNode* &tail){
        if(head == nullptr){
            return true;
        }
        bool ret = isPalindrome(head->next, tail);
        if(ret == false){
            return false;
        }

        if(head->val != tail->val){
            return false;
        }

        tail = tail->next;
        return true;
    }
};
```