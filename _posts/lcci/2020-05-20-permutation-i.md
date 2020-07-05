---
title: 无重复字符串的排列组合
qid: 08.07
tags: [回溯算法]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/permutation-i-lcci/](https://leetcode-cn.com/problems/permutation-i-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>无重复字符串的排列组合。编写一种方法，计算某字符串的所有排列组合，字符串每个字符均不相同。</p>

<p> <strong>示例1:</strong></p>

<pre>
<strong> 输入</strong>：S = "qwe"
<strong> 输出</strong>：["qwe", "qew", "wqe", "weq", "ewq", "eqw"]
</pre>

<p> <strong>示例2:</strong></p>

<pre>
<strong> 输入</strong>：S = "ab"
<strong> 输出</strong>：["ab", "ba"]
</pre>

<p> <strong>提示:</strong></p>

<ol>
<li>字符都是英文字母。</li>
<li>字符串长度在[1, 9]之间。</li>
</ol>


## 解法：

采用回溯法，首先可以在所有字符中选择一个作为第一个字符，然后余下的字符中选择第二个，...直到没得选，这样就得到了一个排列组合了。

而采用迭代的方法，也很简单，考虑字符串 `abc`，如果已经有了 `ab` 的排列组合，那么只需要在 `ab` 的每一个排了组合的每个位置插入字符 `c` 就可以构成一个包含 `c` 的排列组合。


```
ab
ba

cab
acb
abc
cba
bca
bac
```

因此只需要得到一个字符的排列组合，然后在该该排列组合的各个位置插入下一个字符，以此迭代，就能得到整体的排列组合了。

```c++
class Solution {
public:
    vector<string> permutation(string s) {
        vector<string> result;
        result.push_back(s.substr(0, 1));
        for(int i=1;i<s.size();i++){
            int len = result.size();
            vector<string> new_result;
            for(int j=0;j<len;j++){
                const string t = result[j];
                for(int k=0;k<=t.size();k++){
                    string next = t;
                    next.insert(k, 1, s[i]);
                    new_result.push_back(::move(next));
                }
            }
            ::swap(new_result, result);
        }
        return result;
    }
};
```

## 解法二：回溯法

```c++
class Solution {
public:
    vector<string> permutation(string s) {
        vector<string> result;
        vector<bool> visited(s.size(), false);
        string perm;
        dfs(perm, s.size(), s, visited, result);
        return result;
    }

    void dfs(string& perm, int n, const string &s, vector<bool>& visited, vector<string>& result){
        if(n == 0){
            result.push_back(perm);
        }else{
            for(int i = 0;i<s.size();i++){
                if(visited[i]){
                    continue;
                }
                visited[i] = true;
                perm += s[i];
                dfs(perm, n-1, s, visited, result);
                visited[i] = false;
                perm.pop_back();
            }
        }
    }
};
```