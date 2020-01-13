## 22. Generate Parentheses

- 难度： 中等
- 通过率： 52.0%
- 题目链接：[https://leetcode.com/problems/generate-parentheses](https://leetcode.com/problems/generate-parentheses)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>给出&nbsp;<em>n</em>&nbsp;代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且<strong>有效的</strong>括号组合。</p>

<p>例如，给出&nbsp;<em>n </em>=<em> </em>3，生成结果为：</p>

<pre>[
  &quot;((()))&quot;,
  &quot;(()())&quot;,
  &quot;(())()&quot;,
  &quot;()(())&quot;,
  &quot;()()()&quot;
]
</pre>



## 解法：

想了好久没想出来，看了看答案，看到了这个解法。

用一个空字符串，先尝试添加左括号，再添加右括号，利用递归的写法，会优先尝试添加左括号，而后尝试添加右括号，得出的答案符合题目要求。

这个解法可以看做是一种深度优先的搜索，只是在搜索过程中有某些限制条件，即每经过一个左括号，之后可以经过一个右括号。所以需要在搜索的过程中记录下剩余的左括号数，以及当前为封闭的左括号数。


```python
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        results = []
        self.__generateParenthesis(n, 0, '', results)
        return results

    def __generateParenthesis(self, left_remained, left_opened, s, results):
        if left_remained == 0 and left_opened == 0:
            results.append(s)
            return

        if left_remained > 0:
            self.__generateParenthesis(left_remained - 1, left_opened + 1, '(' + s, results)

        if left_opened > 0:
            self.__generateParenthesis(left_remained, left_opened - 1, s + ')', results)
```

2020-1-4 更新

不使用递归求解，消耗内存更小，速度更快。

```cpp
class Solution {
public:
vector<string> generateParenthesis(int n) {
        vector<string> parentheses = {"("};
        vector<int> left_counts = {1};
        vector<string> new_parentheses;
        vector<int> new_left_counts;
        
        for (int i = 1; i < n * 2; i++) {
            new_parentheses.clear();
            new_left_counts.clear();
            new_parentheses.reserve(parentheses.size() * 2);
            new_left_counts.reserve(parentheses.size() * 2);

            for (int j = 0; j < parentheses.size(); j++) {
                string &str = parentheses[j];
                int left = left_counts[j];
                int right = str.size() - left;

                if (left < n) {
                    new_parentheses.emplace_back(str + "(");
                    new_left_counts.emplace_back(left + 1);
                }

                if (left > right) {
                    new_parentheses.emplace_back(str + ")");
                    new_left_counts.emplace_back(left);
                }
            }
            parentheses.swap(new_parentheses);
            left_counts.swap(new_left_counts);
        }
        return parentheses;
    }
};
```