## 56 - I. 数组中数字出现的次数

- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>一个整型数组 <code>nums</code> 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>nums = [4,1,4,6]
<strong>输出：</strong>[1,6] 或 [6,1]
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>nums = [1,2,10,4,1,4,3,3]
<strong>输出：</strong>[2,10] 或 [10,2]</pre>

<p>&nbsp;</p>

<p><strong>限制：</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10000</code></li>
</ul>

<p>&nbsp;</p>


## 解法：

如果只有一个出现一次的数，把所有数字异或起来就可以得到结果。因为 `n ^ n = 0`，`0 ^ n = n`。因此出现两次的数，两者异或后结果为 `0`，`0 ^ n = n`，因此最终结果就是那个出现一次的数。

但是次数出现一次的数有两个，设为 `a` 和 `b`，全部异或后得到的结果为 `a^b`。

在 `a^b` 的结果中，假设二进制最低位为 `1`，就说明 `a` 和 `b` 的最低位是不同的。为此，在异或的过程中，可以把最低位为 `1` 的异或在一次，为 `0` 的异或在一起。这样 `a` 和 `b` 不就分开了吗。

因此，第一步还是一样的，把全部数字异或起来，得到 `a^b`，然后找到 `a^b` 中某个为 `1` 的位。而后再次遍历数组，根据情况把它们异或在一起。


```c++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int a_xor_b = 0;
        for(int n: nums){
            a_xor_b ^= n;
        }

        int i = a_xor_b & (-a_xor_b);
        int a = 0, b = 0;
        for(int n: nums){
            if(n & i){
                a ^= n;
            }else{
                b ^= n;
            }
        }
        return vector<int>{a, b};
    }
};
```

上面的寻找 `a_xor_b` 中为 `1` 的位采用的方法很巧妙，`a_xor_b & (-a_xor_b)`。

因为 `-a` 的二进制表示为 `~(a - 1)`。即，把 a 减去 1，然后整体取反。因此 `a & (-a) = a & ~(a-1)`。

假设 `a = 0b10 0100`，那么：

```
a        = 0b10 0100

a-1      = 0b10 0011
~(a-1)   = 0b01 1100  (-a)

a & (-a) = 0x00 0100
```

发现了吗， `a & (-a)` 一下子就能得到 `a` 中为 `1` 的最低那一位。