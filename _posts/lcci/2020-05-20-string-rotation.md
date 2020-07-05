---
title: 字符串轮转
qid: 01.09
tags: [字符串]
---


- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/string-rotation-lcci/](https://leetcode-cn.com/problems/string-rotation-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>字符串轮转。给定两个字符串<code>s1</code>和<code>s2</code>，请编写代码检查<code>s2</code>是否为<code>s1</code>旋转而成（比如，<code>waterbottle</code>是<code>erbottlewat</code>旋转后的字符串）。</p>

<p><strong>示例1:</strong></p>

<pre><strong> 输入</strong>：s1 = &quot;waterbottle&quot;, s2 = &quot;erbottlewat&quot;
<strong> 输出</strong>：True
</pre>

<p><strong>示例2:</strong></p>

<pre><strong> 输入</strong>：s1 = &quot;aa&quot;, &quot;aba&quot;
<strong> 输出</strong>：False
</pre>

<ol>
</ol>

<p><strong>提示：</strong></p>

<ol>
	<li>字符串长度在[0, 100000]范围内。</li>
</ol>

<p><strong>说明:</strong></p>

<ol>
	<li>你能只调用一次检查子串的方法吗？</li>
</ol>


## 解法：

```c++
class Solution {
public:
    bool isFlipedString(string s1, string s2) {
        if(s1.size() != s2.size()){
            return false;
        }

        s1 += s1;
        return s1.find(s2) != -1;
    }
};
```