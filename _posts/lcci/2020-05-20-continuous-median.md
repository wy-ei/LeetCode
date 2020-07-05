---
title: 连续中值
qid: 17.20
tags: [堆]
---


- 难度：Hard
- 题目链接：[https://leetcode-cn.com/problems/continuous-median-lcci/](https://leetcode-cn.com/problems/continuous-median-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>随机产生数字并传递给一个方法。你能否完成这个方法，在每次产生新值时，寻找当前所有值的中间值（中位数）并保存。</p>

<p>中位数是有序列表中间的数。如果列表长度是偶数，中位数则是中间两个数的平均值。</p>

<p>例如，</p>

<p>[2,3,4]&nbsp;的中位数是 3</p>

<p>[2,3] 的中位数是 (2 + 3) / 2 = 2.5</p>

<p>设计一个支持以下两种操作的数据结构：</p>

<ul>
	<li>void addNum(int num) - 从数据流中添加一个整数到数据结构中。</li>
	<li>double findMedian() - 返回目前所有元素的中位数。</li>
</ul>

<p><strong>示例：</strong></p>

<pre>addNum(1)
addNum(2)
findMedian() -&gt; 1.5
addNum(3) 
findMedian() -&gt; 2
</pre>


## 解法：

使用一个大顶堆和一个小顶堆，分别存放所有的数中小的一半和大的一半。小的一半的最大值和大的一半的最小值，就是构成中位数的两个数了，这正是两个堆顶元素。

```c++
class MedianFinder {
public:
    void addNum(int num) {
        if(max_pq.size() > min_pq.size()){
            max_pq.push(num);
            min_pq.push(max_pq.top());
            max_pq.pop();
        }else{
            min_pq.push(num);
            max_pq.push(min_pq.top());
            min_pq.pop();
        }
    }

    double findMedian() {
        if(max_pq.size() > min_pq.size()){
            return max_pq.top();
        }else{
            double sum = max_pq.top() + min_pq.top();
            return sum / 2;
        }
    }
private:
    priority_queue<int, vector<int>, greater<>> min_pq;
    priority_queue<int> max_pq;
};
```

- 大顶堆：每个结点的值都大于或等于其左右孩子结点的值
- 小顶堆：每个结点的值都小于或等于其左右孩子结点的值

`priority_queue<int>` 是大顶堆，即堆顶的元素的值最大。