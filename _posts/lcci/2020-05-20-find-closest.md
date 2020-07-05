---
title: 单词距离
qid: 17.11
tags: [双指针,字符串]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/find-closest-lcci/](https://leetcode-cn.com/problems/find-closest-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>有个内含单词的超大文本文件，给定任意两个单词，找出在这个文件中这两个单词的最短距离(相隔单词数)。如果寻找过程在这个文件中会重复多次，而每次寻找的单词不同，你能对此优化吗?</p>

<p><strong>示例：</strong></p>

<pre><strong>输入：</strong>words = [&quot;I&quot;,&quot;am&quot;,&quot;a&quot;,&quot;student&quot;,&quot;from&quot;,&quot;a&quot;,&quot;university&quot;,&quot;in&quot;,&quot;a&quot;,&quot;city&quot;], word1 = &quot;a&quot;, word2 = &quot;student&quot;
<strong>输出：</strong>1</pre>

<p>提示：</p>

<ul>
	<li><code>words.length &lt;= 100000</code></li>
</ul>


## 解法：

单次运行，下面这样的方法就可以了。

```cpp
class Solution {
public:
    int findClosest(vector<string>& words, string word1, string word2) {
        int pos1 = -1, pos2 = -1;
        int min_distance = words.size();
        for(int i=0;i<words.size();i++){
            if(words[i] == word1){
                pos1 = i;
                if(pos2 != -1){
                    min_distance = min(min_distance, abs(pos1 - pos2));
                }
            }else if(words[i] == word2){
                pos2 = i;
                if(pos1 != -1){
                    min_distance = min(min_distance, abs(pos1 - pos2));
                }
            }
        }
        return min_distance;
    }
};
```

如果要多次运行，那可以先把所有单词出现的下标统计出来。然后给定两个单词，在两个下标数组中，寻找距离最近的一对下标即可。

在 (16.06. 最小差)[.\16.06-smallest-difference-lcci.md] 中给出了找两个数组中最相近的两个元素的方法。

```cpp
class Text {
public:
    Text(vector<string>& words){
        for(int i=0;i<words.size();i++){
            mp[words[i]].push_back(i);
        } 
    }
    int findClosest(string word1, string word2) {
        vector<int> a = mp[word1];
        vector<int> b = mp[word2];
        int i = 0, j = 0;

        int min_distance = words.size();
        while(i < a.size() && j < b.size()){
            min_distance = min(min_distance, abs(a[i] - b[j]));
            if(a[i] < b[j]){
                i++;
            }else{
                j++;
            }
        }
        return min_distance;
    }
private:
    unordered_map<string, vector<int>> mp;
};
```