---
title: 三合一
qid: 03.01
tags: [设计]
---


- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/three-in-one-lcci/](https://leetcode-cn.com/problems/three-in-one-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>三合一。描述如何只用一个数组来实现三个栈。</p>

<p>你应该实现<code>push(stackNum, value)</code>、<code>pop(stackNum)</code>、<code>isEmpty(stackNum)</code>、<code>peek(stackNum)</code>方法。<code>stackNum</code>表示栈下标，<code>value</code>表示压入的值。</p>

<p>构造函数会传入一个<code>stackSize</code>参数，代表每个栈的大小。</p>

<p><strong>示例1:</strong></p>

<pre><strong> 输入</strong>：
[&quot;TripleInOne&quot;, &quot;push&quot;, &quot;push&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;isEmpty&quot;]
[[1], [0, 1], [0, 2], [0], [0], [0], [0]]
<strong> 输出</strong>：
[null, null, null, 1, -1, -1, true]
<strong>说明</strong>：当栈为空时`pop, peek`返回-1，当栈满时`push`不压入元素。
</pre>

<p><strong>示例2:</strong></p>

<pre><strong> 输入</strong>：
[&quot;TripleInOne&quot;, &quot;push&quot;, &quot;push&quot;, &quot;push&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;pop&quot;, &quot;peek&quot;]
[[2], [0, 1], [0, 2], [0, 3], [0], [0], [0], [0]]
<strong> 输出</strong>：
[null, null, null, null, 2, 1, -1, -1]
</pre>


## 解法：

题目中要求在一个数组中实现三个栈，常规的方法是把数组分成三等份，然后在不同的范围内各自实现一个栈。

这里我在数组中基于下标实现了三个链表，每个栈对应一个链表，push 操作就是在链表头部加入一个节点，pop 就是删除头部节点。数组中未被使用的元素都由 freelist 维护，每次 push 的时候就从 freelist 哪里得到一个空闲节点，pop 的时候再还回去。

数组中连续两个元素构成一个节点，`a[i*2+0]` 中为下一个节点的下标。`a[i*2+1]` 为当前节点的值。三个栈对应的链表的表头，使用数组中前三个单元。

```c++
class TripleInOne {
public:
    TripleInOne(int stackSize):size(stackSize) {
        arr.resize((stackSize+1)*3*2, -1);
        freelist = 3;
        arr[1] = 0;
        arr[3] = 0;
        arr[5] = 0;
    }

    void push(int stackNum, int value) {
        int i = freelist;
        if(freelist * 2 >= arr.size()){
            // full
            return;
        }
        // 删掉下面这个判断，可以让一个栈利用余下的空间，并超过栈的大小限制。
        if(arr[stackNum*2+1] >= size){
            return;
        }
        if(arr[freelist*2] == -1){
            freelist += 1;
        }else{
            freelist = arr[freelist*2];
        }
        arr[i*2] = arr[stackNum*2];
        arr[i*2+1] = value;
        arr[stackNum*2] = i;
        arr[stackNum*2+1] += 1;
    }

    int pop(int stackNum) {
        if(arr[stackNum*2+1] == 0){
            return -1;
        }
        arr[stackNum*2+1] -= 1;
        int i = arr[stackNum*2];
        arr[stackNum*2] = arr[i*2];
        arr[i*2] = freelist;
        freelist = i;
        return arr[i*2+1];
    }

    int peek(int stackNum) {
        if(arr[stackNum*2] == -1){
            return -1;
        }
        int i = arr[stackNum*2];
        return arr[i*2+1];
    }

    bool isEmpty(int stackNum) {
        return arr[stackNum*2] == -1;
    }

private:
    vector<int> arr;
    int freelist;
    int size;
};
```