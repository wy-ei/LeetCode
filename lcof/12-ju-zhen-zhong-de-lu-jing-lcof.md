## 12. 矩阵中的路径

- 难度：Medium
- 题目链接：[https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)


## 题目描述

来源于 [https://leetcode-cn.com/](https://leetcode-cn.com/)

<p>请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3&times;4的矩阵中包含一条字符串&ldquo;bfce&rdquo;的路径（路径中的字母用加粗标出）。</p>

<p>[[&quot;a&quot;,&quot;<strong>b</strong>&quot;,&quot;c&quot;,&quot;e&quot;],<br>
[&quot;s&quot;,&quot;<strong>f</strong>&quot;,&quot;<strong>c</strong>&quot;,&quot;s&quot;],<br>
[&quot;a&quot;,&quot;d&quot;,&quot;<strong>e</strong>&quot;,&quot;e&quot;]]</p>

<p>但矩阵中不包含字符串&ldquo;abfb&rdquo;的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。</p>

<p>&nbsp;</p>

<p><strong>示例 1：</strong></p>

<pre><strong>输入：</strong>board = [[&quot;A&quot;,&quot;B&quot;,&quot;C&quot;,&quot;E&quot;],[&quot;S&quot;,&quot;F&quot;,&quot;C&quot;,&quot;S&quot;],[&quot;A&quot;,&quot;D&quot;,&quot;E&quot;,&quot;E&quot;]], word = &quot;ABCCED&quot;
<strong>输出：</strong>true
</pre>

<p><strong>示例 2：</strong></p>

<pre><strong>输入：</strong>board = [[&quot;a&quot;,&quot;b&quot;],[&quot;c&quot;,&quot;d&quot;]], word = &quot;abcd&quot;
<strong>输出：</strong>false
</pre>

<p><strong>提示：</strong></p>

<ul>
	<li><code>1 &lt;= board.length &lt;= 200</code></li>
	<li><code>1 &lt;= board[i].length &lt;= 200</code></li>
</ul>

<p>注意：本题与主站 79 题相同：<a href="https://leetcode-cn.com/problems/word-search/">https://leetcode-cn.com/problems/word-search/</a></p>


## 解法：

本题我感觉是深度优先搜索的问题，从矩阵中某个位置开始向四面八方进行搜索。对于处理过的位置，使用特殊符号标记。由于采用递归的写法，先前访问过的位置会被标记，深层递归能够感知到被标记的位置。


```cpp
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
		int n_rows = board.size();
		int n_cols = board[0].size();

		for(int row=0;row<n_rows;row++){
			for(int col=0;col<n_cols;col++){
				bool ret = exist(board, word, 0, row, col);
				if(ret){
					return true;
				}
			}
		}
		return false;
    }

private:
	bool exist(vector<vector<char>>& board, const string& word, int i, int row, int col){
		if(i == word.size()){
			return true;
		}

		int n_rows = board.size();
		int n_cols = board[0].size();

		if(row == n_rows || row < 0 || col == n_cols || col < 0){
			return false;
		}
		char ch = board[row][col];

		if(ch != word[i]){
			return false;
		}

		board[row][col] = '*';
		
		bool is_exist = exist(board, word, i+1, row-1, col) ||
			exist(board, word, i+1, row+1, col) ||
			exist(board, word, i+1, row, col-1) ||
			exist(board, word, i+1, row, col+1);

		board[row][col] = ch;

		return is_exist;
	}
};
```