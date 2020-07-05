---
title: 变位词组
qid: 10.02
tags: [哈希表,字符串]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/group-anagrams-lcci/](https://leetcode-cn.com/problems/group-anagrams-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>编写一种方法，对字符串数组进行排序，将所有变位词组合在一起。变位词是指字母相同，但排列不同的字符串。</p>

<p><strong>注意：</strong>本题相对原题稍作修改</p>

<p><strong>示例:</strong></p>

<pre><strong>输入:</strong> <code>[&quot;eat&quot;, &quot;tea&quot;, &quot;tan&quot;, &quot;ate&quot;, &quot;nat&quot;, &quot;bat&quot;]</code>,
<strong>输出:</strong>
[
  [&quot;ate&quot;,&quot;eat&quot;,&quot;tea&quot;],
  [&quot;nat&quot;,&quot;tan&quot;],
  [&quot;bat&quot;]
]</pre>

<p><strong>说明：</strong></p>

<ul>
	<li>所有输入均为小写字母。</li>
	<li>不考虑答案输出的顺序。</li>
</ul>


## 解法：

把字符串排序作为键，每个键管理一个数组。把排序后相同的字符串插入到同一个数组中。

我觉得把字符串排序作为键，性能可能不够好。但是如何把字符构成相同的字符串映射为一个相同的值，我一时半会想不到更佳的策略。我知道一直方法，可以把字符串 `ababa` 编码为 `a3b2`，如果字符串足够长，这种方法会更好一些。

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> code_to_strings;
        for(auto& s: strs){
            string t = s;
            sort(t.begin(), t.end());
            code_to_strings[t].push_back(s);
        }
        vector<vector<string>> ret;
        ret.reserve(code_to_strings.size());
        for(auto& item: code_to_strings){
            ret.emplace_back(::move(item.second));
        }
        return ret;
    }
};
```