---
title: 下一个数
qid: 05.04
tags: [位运算]
---


- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/closed-number-lcci/](https://leetcode-cn.com/problems/closed-number-lcci/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>下一个数。给定一个正整数，找出与其二进制表达式中1的个数相同且大小最接近的那两个数（一个略大，一个略小）。</p>

<p> <strong>示例1:</strong></p>

<pre>
<strong> 输入</strong>：num = 2（或者0b10）
<strong> 输出</strong>：[4, 1] 或者（[0b100, 0b1]）
</pre>

<p> <strong>示例2:</strong></p>

<pre>
<strong> 输入</strong>：num = 1
<strong> 输出</strong>：[2, -1]
</pre>

<p> <strong>提示:</strong></p>

<ol>
<li><code>num</code>的范围在[1, 2147483647]之间；</li>
<li>如果找不到前一个或者后一个满足条件的正数，那么输出 -1。</li>
</ol>


## 解法：

设 `num = 1,837,591,841`，num 的二进制表示如下：

```
110 1101 1000 0111 0110 1101 0010 0001‬
```

把它视为一个数组，下一个较大的数的各个位，就是这个数组的下一个排列组合。

而前一个较小的数，就是这个数组的前一个排列组合。寻找前一个排列组合，只需要修改一下比较函数即可。

寻找下一个排列组合的算法可以参考：leetcode 31 题。


```c++
class Solution {
public:
    vector<int> findClosedNumbers(int num) {
        return {next(num) , prev(num)};
    }

    int next(int num){
        vector<char> bits;
        for(int i=31;i>=0;i--){
            bits.push_back(bit(num, i));
        }

        if(next_permutation(bits.begin(), bits.end())){
            int n = 0;
            for(int i=0;i<bits.size();i++){
                if(bits[i] == 1){
                    n = set_bit(n, 31-i, 1);
                }
            }
            return n;
        }else{
            return -1;
        }
    }

    int prev(int num){
        vector<char> bits;
        for(int i=31;i>=0;i--){
            bits.push_back(bit(num, i));
        }
        auto comp = [](int a, int b){
            return a > b;
        };
        if(next_permutation(bits.begin(), bits.end(), comp)){
            int n = 0;
            for(int i=0;i<bits.size();i++){
                if(bits[i] == 1){
                    n = set_bit(n, 31-i, 1);
                }
            }
            return n;
        }else{
            return -1;
        }
    }

    int bit(int num, int i){
        if(num & (1 << i)){
            return 1;
        }else{
            return 0;
        }
    }

    int set_bit(int num, int i, int val){
        if(val){
            return num | (1 << i);
        }else{
            return num & ~(1 << i);
        }
    }
};
```