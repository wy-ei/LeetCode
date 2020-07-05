---
title: 不同路径 II
qid: 63
tags: [数组,动态规划]
---


- 难度： 中等
- 通过率： 33.0%
- 题目链接：[https://leetcode.com/problems/unique-paths-ii](https://leetcode.com/problems/unique-paths-ii)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>一个机器人位于一个 <em>m x n </em>网格的左上角 （起始点在下图中标记为&ldquo;Start&rdquo; ）。</p>

<p>机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为&ldquo;Finish&rdquo;）。</p>

<p>现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？</p>

<p><img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/robot_maze.png" style="height: 183px; width: 400px;"></p>

<p>网格中的障碍物和空位置分别用 <code>1</code> 和 <code>0</code> 来表示。</p>

<p><strong>说明：</strong><em>m</em>&nbsp;和 <em>n </em>的值均不超过 100。</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:
</strong>[
&nbsp; [0,0,0],
&nbsp; [0,1,0],
&nbsp; [0,0,0]
]
<strong>输出:</strong> 2
<strong>解释:</strong>
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 <code>2</code> 条不同的路径：
1. 向右 -&gt; 向右 -&gt; 向下 -&gt; 向下
2. 向下 -&gt; 向下 -&gt; 向右 -&gt; 向右
</pre>


## 解法：

只需要在 [上一题](./062-unique-paths.md) 的基础上稍加修改即可。

```python
from collections import defaultdict

class Solution:
    def uniquePathsWithObstacles(self, obstacle_grid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        
        m = len(obstacle_grid)
        n = len(obstacle_grid[0])
        
        dp = defaultdict(int)
        dp[(m-1,n-1)] = 1
        
        for col in range(n-1, -1, -1):
            for row in range(m-1, -1, -1):
                if obstacle_grid[row][col] == 1:
                    dp[(row, col)] = 0
                    continue
                
                if row < m-1:
                    dp[(row, col)] += dp[(row+1, col)]
                
                if col < n-1:
                    dp[(row, col)] += dp[(row, col+1)]
        
        return dp[(0,0)]
```