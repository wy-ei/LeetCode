---
title: 最后一个单词的长度
qid: 58
tags: [字符串]
---



- 难度： 简单
- 通过率： 32.1%
- 题目链接：[https://leetcode-cn.com/problems/length-of-last-word](https://leetcode-cn.com/problems/length-of-last-word)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给定一个仅包含大小写字母和空格&nbsp;<code>&#39; &#39;</code>&nbsp;的字符串，返回其最后一个单词的长度。</p>

<p>如果不存在最后一个单词，请返回 0&nbsp;。</p>

<p><strong>说明：</strong>一个单词是指由字母组成，但不包含任何空格的字符串。</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> &quot;Hello World&quot;
<strong>输出:</strong> 5
</pre>


## 解法：

此题很容易，无需多言。

```python
class Solution:
    def lengthOfLastWord(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        i = len(s) - 1

        while i >= 0 and s[i] == ' ':
            i -= 1
        
        length = 0
        while i >= 0 and s[i] != ' ':
            i -= 1
            length + = 1

        return length
```