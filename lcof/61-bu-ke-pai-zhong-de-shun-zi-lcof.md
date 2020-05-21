## 61. 扑克牌中的顺子

- 难度：Easy
- 题目链接：[https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。</p>

<p>&nbsp;</p>

<p><strong>示例&nbsp;1:</strong></p>

<pre><strong>输入:</strong> [1,2,3,4,5]
<strong>输出:</strong> True</pre>

<p>&nbsp;</p>

<p><strong>示例&nbsp;2:</strong></p>

<pre><strong>输入:</strong> [0,0,1,2,5]
<strong>输出:</strong> True</pre>

<p>&nbsp;</p>

<p><strong>限制：</strong></p>

<p>数组长度为 5&nbsp;</p>

<p>数组的数取值为 [0, 13] .</p>


## 解法：

先排序，然后统计王的个数，以及其他牌中空位个数。比如 `1 2 3 4` 没有空位，`1 2 4 5` 缺少 3 才构成顺序，空位为 1。最后判断王是否能够填充空位。

```c++
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        int n_joker = 0, n_slot = 0;

        for(int i = 0; i < nums.size(); i++){
            if(nums[i] == 0){
                n_joker += 1;
                continue;
            }
            if(i > 0 && nums[i] == nums[i-1]){
                return false;
            }

            if(i > 0 && nums[i-1] != 0){
                n_slot += nums[i] - nums[i-1] - 1;
            }
        }

        return n_joker >= n_slot;
    }
};
```