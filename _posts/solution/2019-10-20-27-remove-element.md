---
title: 移除元素
qid: 27
tags: [数组,双指针]
---


- 难度： 简单
- 通过率： 42.9%
- 题目链接：[https://leetcode-cn.com/problems/remove-element](https://leetcode-cn.com/problems/remove-element)


## 题目描述

<p>给定一个数组 <em>nums&nbsp;</em>和一个值 <em>val</em>，你需要<strong><a href="https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95" target="_blank">原地</a></strong>移除所有数值等于&nbsp;<em>val&nbsp;</em>的元素，返回移除后数组的新长度。</p>

<p>不要使用额外的数组空间，你必须在<strong><a href="https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95" target="_blank">原地</a>修改输入数组</strong>并在使用 O(1) 额外空间的条件下完成。</p>

<p>元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。</p>

<p><strong>示例 1:</strong></p>

<pre>给定 <em>nums</em> = <strong>[3,2,2,3]</strong>, <em>val</em> = <strong>3</strong>,

函数应该返回新的长度 <strong>2</strong>, 并且 <em>nums </em>中的前两个元素均为 <strong>2</strong>。

你不需要考虑数组中超出新长度后面的元素。
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre>给定 <em>nums</em> = <strong>[0,1,2,2,3,0,4,2]</strong>, <em>val</em> = <strong>2</strong>,

函数应该返回新的长度 <strong><code>5</code></strong>, 并且 <em>nums </em>中的前五个元素为 <strong><code>0</code></strong>, <strong><code>1</code></strong>, <strong><code>3</code></strong>, <strong><code>0</code></strong>, <strong>4</strong>。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。
</pre>

<p><strong>说明:</strong></p>

<p>为什么返回数值是整数，但输出的答案是数组呢?</p>

<p>请注意，输入数组是以<strong>&ldquo;引用&rdquo;</strong>方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。</p>

<p>你可以想象内部操作如下:</p>

<pre>// <strong>nums</strong> 是以&ldquo;引用&rdquo;方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中<strong>该长度范围内</strong>的所有元素。
for (int i = 0; i &lt; len; i++) {
&nbsp; &nbsp; print(nums[i]);
}
</pre>


## 解法 1：

简单，但缺点是容易出现没必要的赋值。

```python
def removeElement(nums, val):
    """
    :type nums: List[int]
    :type val: int
    :rtype: int
    """
    i = 0
    for num in nums:
        if num != val:
            nums[i] = num
            i += 1
    return i
```

## 解法 2：

看看别人写的代码，真是自愧不如。遇到 val 就把数组最后一个元素换过来，然后缩短数组长度。

```python
def removeElement(self, nums, val):
    i = 0
    n = len(nums)
    while i < n:
        if nums[i] == val:
            nums[i] = nums[n-1]
            n -= 1
        else:
            i += 1
    return n
```