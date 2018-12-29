## 10. Regular Expression Matching


- 难度： 困难
- 通过率： 24.7%
- 题目链接：[https://leetcode.com/problems/regular-expression-matching](https://leetcode.com/problems/regular-expression-matching)


### 解法 1

使用动态规划，利用一个二维数组来记录模式与字符串的匹配情况，具体说来 `match[i][j] == True` 表示 `s[0:i]` 与 `p[0:j]` 匹配。

```python
1. 当 s[j] == p[j] or p[j] == '.' 时，有 match[i+1][j+1] = match[i][j]
2. 当 p[j] == '*' 时，分两种情况：
  1. p[j-1] == s[i] or p[j-1] == '.'，即 `*` 前面的字符与 `s[i]` 匹配，那么加一个 `*` 可以表示重复1次，重复 0 次，重复多次，此时有：
      match[i+1][j+1] = match[i+1][j]   # 表示 `*` 将前面字符重复一次
                     or match[i+1][j-1] # 表示 `*` 将前面字符重复零次
                     or match[i][j+1]   # 表示 `*` 将前面字符重复多次，就是说如果 p[0:j+1] 能匹配 s[0:i]，
                                        # 说明这个 `*` 已经与 s[i-1] 匹配了，那么 s[i-1] 再重复一次，也能匹配上。
                
      # 只要前面匹配了，那么多个 *  就可以通过重复 0 次、1 次、多次继续进行匹配
    
  2. p[j-1] != s[i]，表示 `*` 前面的字符与 `s[i]` 不匹配，那么加一个 `*` 可以表示重复 0 次。
      match[i+1][j+1] = match[i+1][j-1] # 表示 `*` 将前面字符重复零次
```

代码如下：

```python
class Solution:
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        m = len(s)
        n = len(p)
        match = [[False for _ in range(n + 1)] for __ in range(m + 1)]
        match[0][0] = True

        # 处理空字符串的情况
        # 对于空字符串，模式也需要能够匹配
        for i in range(n):
            if p[i] == '*' and match[0][i-1]:
                p[0][i+1] = True

        for i in range(m):
            for j in range(n):
                if p[j] == s[i] or p[j] == '.':
                    match[i+1][j+1] = match[i][j]
                if p[j] == '*':
                    if p[j - 1] == s[i] or p[j - 1] == '.':
                        match[i+1][j+1] = match[i+1][j] or match[i+1][j-1] or match[i][j+1]
                    else:
                        match[i+1][j+1] = match[i+1][j-1]

        return match[len(s)][len(p)]
```

### 解法 2

另外可以使用 NFA（非确定有限状态机）来解决，参加普林斯顿出的算法（第四版）

```python

class Digraph:
    def __init__(self, V):
        self.__V = V
        self.__E = 0
        self.__adj = [set() for _ in range(V)]

    def add_edge(self, v, w):
        self.__adj[v].add(w)
        self.__E += 1

    def V(self):
        return self.__V

    def E(self):
        return self.__E

    def adj(self, v):
        return self.__adj[v]

from collections import Iterable

class DirectedDFS:
    def __init__(self, graph, source):
        self.__marked = [0] * graph.V()
        if isinstance(source, Iterable):
            for s in source:
                self.dfs(graph, s)
        else:
            self.dfs(graph, source)

    def dfs(self, graph, v):
        self.__marked[v] = 1
        for w in graph.adj(v):
            if self.__marked[w] == 0:
                self.dfs(graph, w)

    def marked(self, v):
        return self.__marked[v]


# 非确定有限自动机
def NFA(regexp):
    M = len(regexp)
    g = Digraph(M + 1)
    for i, c in enumerate(regexp):
        if i < M - 1 and regexp[i+1] == '*':
            g.add_edge(i, i+1)
            g.add_edge(i+1, i)

        if regexp[i] == '*':
            g.add_edge(i, i+1)
    return g


def recognizes(txt, regexp):
    digraph = NFA(regexp)
    dfs = DirectedDFS(digraph, 0)
    pc = set()

    for v in range(digraph.V()):
        if dfs.marked(v):
            pc.add(v)

    for i, c in enumerate(txt):
        match = set()

        for v in pc:
            if v < len(regexp):
                if regexp[v] == txt[i] or regexp[v] == '.':
                    match.add(v+1)

        pc = set()
        dfs = DirectedDFS(digraph, match)
        for v in range(digraph.V()):
            if dfs.marked(v):
                pc.add(v)

    for v in pc:
        if v == len(regexp):
            return True

    return False

```