## 50. Pow(x, n)

- 难度： 中等
- 通过率： 27.2%
- 题目链接：[https://leetcode.com/problems/powx-n](https://leetcode.com/problems/powx-n)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>实现&nbsp;<a href="https://www.cplusplus.com/reference/valarray/pow/" target="_blank">pow(<em>x</em>, <em>n</em>)</a>&nbsp;，即计算 x 的 n 次幂函数。</p>

<p><strong>示例 1:</strong></p>

<pre><strong>输入:</strong> 2.00000, 10
<strong>输出:</strong> 1024.00000
</pre>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> 2.10000, 3
<strong>输出:</strong> 9.26100
</pre>

<p><strong>示例&nbsp;3:</strong></p>

<pre><strong>输入:</strong> 2.00000, -2
<strong>输出:</strong> 0.25000
<strong>解释:</strong> 2<sup>-2</sup> = 1/2<sup>2</sup> = 1/4 = 0.25</pre>

<p><strong>说明:</strong></p>

<ul>
	<li>-100.0 &lt;&nbsp;<em>x</em>&nbsp;&lt; 100.0</li>
	<li><em>n</em>&nbsp;是 32 位有符号整数，其数值范围是&nbsp;[&minus;2<sup>31</sup>,&nbsp;2<sup>31&nbsp;</sup>&minus; 1] 。</li>
</ul>


### 非递归解法：

求解一个数的 N 次幂，存在一个递归解法，但显然不如下面这个方法好。

```python
class Solution:
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        sign = 1 if n >= 0 else -1
        n = abs(n)

        w = x
        ans = 1

        while n:
            if (n & 1) == 1:
                ans *= w
            n = n >>  1
            w = w * w
        
        if sign == -1:
            return 1 / ans
        else:
            return ans
```

解释如下：

举个例子，假如希望求 `5^62`，也就是 5 的 62 次方。为了算法高效，一个原则就是不做重复的计算。

```c
5^62 = 5^(32+16+8+4+2) = 5^32 * 5^16 * 5^8 * 5^4 * 5^2
```

不管指数是多少，都可以将其分解为 2 的倍数的和，因为任何整数都能够写成 2 进制的形式，比如 `62 = 00111110B`。

以上算法中，随着迭代 `w` 的值会变成 `x`, `x^2`, `x^4`, `x^8`,...，我们只需要在合适的时候让它和 ans 相乘即可。合适的时刻就是 N 的二进制表示的相应位上为 1 的时候，这里使用了右移，只需要判断最低位是不是 1 就好了。

对于负的幂次，可以先求正幂次的结果，然后取倒数。

这个算法时间复杂度为 `O(logN)` ，这里的 N 是指待求幂次的大小，因此这算法算是一个伪多项式算法。