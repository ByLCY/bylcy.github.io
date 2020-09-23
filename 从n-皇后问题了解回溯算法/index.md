# 从N 皇后问题了解回溯算法


<!--more-->

## N皇后问题

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。[N 皇后练习题](https://leetcode-cn.com/problems/n-queens/)

{{<image src="/images/从N-皇后问题了解回溯算法/8-queens.png" width="300px" caption="八皇后的一种解法">}}

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

```Markdown
// 力扣的例子
输入：4
输出：[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。
```

在国际象棋中皇后可以攻击八个方向上的敌人（上下左右、左上、右上、左下、右下）。

## 题解

### 深度优先搜索

根据题目可知道每一行只会有一个皇后，每个对角线上只有一个皇后，由此可以进行编码。首先大体思路是使用深度优先遍历所有可能位置，然后根据限制条件检查位置是否合适。

```C++
for (int row = 0; row < n; row++) {
    for (int col = 0; col < n; col++) {
        if (check(棋盘[i][j])) {
            将当前位置保存;
            到下一行检查是否有列合适;
            恢复到本次更改;
        }
    }
}
```

对上述伪代码进行优化得到如下代码：

```C++
void dfs(int n, int row, vector<string>& ans, vector<vector<string>> res) {
    // n为棋盘大小
    // row为当前行
    // ans 为保存了到目前为止推算出的皇后位置
    // res保存答案
    if (row == n) {
        res.push_back(ans);
        return;
    }
    for (int col = 0; col < n; col++) {
        if (check(row, col, n, ans)) {
            ans[row][col] = 'Q';
            dfs(n, row + 1, ans);
            ans[row][col] = '.';  //  此步进行回溯
        }
    }
}
```

### 通过条件剪支

然后就是解决check函数。根据题目的要求我们要检测皇后是否在同一列或同一条对角线上（之所以不用检查是否在同一行上，因为根据我们上面的函数我们是不可能同一时间在同一行上放两个皇后的）。由此我们可以提出两种思路，第一种是：每当准备安放皇后时就通过行和列来回推同一列和同一对角线的位置是否有皇后。第二种思路就是通过观察行和对角线的下标规律来检测是否满足条件。

通过第一种思路我们可以写出如下代码：

```C++
bool check(int row, int col, int n, vector<string>& ans) {
    // 检查之前的列是否存在皇后
    for(int i = 0; i < row; i++) {
        if (ans[i][col] == 'Q') {
            return false;
        }
    }
	// 检查从左上到右下对角线是否存在皇后
    for(int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (ans[i][j] == 'Q') {
            return false;
        }
    }
	// 检查从右上到左下对角线是否存在皇后
    for(int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (ans[i][j] == 'Q') {
            return false;
        }
    }

    return true;
}
```

第二种思路的规律就是在同一列只会存在一个皇后，所以只要保存有皇后的列下标的集合就行了。而从左上到右下的对角线的行减去列的值相等，所以保存差的集合。最后右上到左下的对角线的行加上列的值相等，所以保存和的集合。一旦集合有重复就说明皇后的位置出错。代码如下：

```C++
void backtrack(vector<vector<string>>& res, vector<string>& ans, unordered_set<int>& cls， unordered_set<int>& diagonalLR, unordered_set<int>& diagonalRL, int n, int row) {
    // 有皇后的集合
    // cls列
    // diagonalLR对角线左上到右下
    // diagonalRL对角线右上到左下
    if (n == row) {  // 皇后的位置摆放完毕，保存当前解法
        res.push_back(ans);
        return;
    }
    //  如果没有摆放完毕
    for (int i = 0; i < n; i++) {  //  循环列
        int dlr = row - i;
        int drl = row + i;
        if(cls.find(i) != cls.end() ||
        diagonalLR.find(dlr) != diagonalLR.end() ||
        diagonalRL.find(drl) != diagonalRL.end()) {
            continue;  //  第row列i行会被其他皇后攻击到，跳过
        }
        cls.insert(i);
        diagonalLR.insert(dlr);
        diagonalRL.insert(drl);
        ans[row][i] = 'Q';
        backtrack(ans, queens, cls, diagonalLR, diagonalRL, n, row + 1);
        //  回溯
        cls.erase(i);
        diagonalLR.erase(dlr);
        diagonalRL.erase(drl);
        ans[row][i] = '.';
    }
}
```

### 答案

最后通过两种方法得到的解答如下：

```C++
// 第一种
class Solution {
public:
    bool check(int row, int col, int n, vector<string>& ans) {
        for(int i = 0; i < row; i++) {
            if (ans[i][col] == 'Q') {
                return false;
            }
        }

        for(int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (ans[i][j] == 'Q') {
                return false;
            }
        }

        for(int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (ans[i][j] == 'Q') {
                return false;
            }
        }

        return true;
    }

    void dfs(int n, int row, vector<string>& ans, vector<vector<string>>& res) {
        if (row == n) {
            res.push_back(ans);
            return;
        }
        for (int col = 0; col < n; col++) {
            if (check(row, col, n, ans)) {
                ans[row][col] = 'Q';
                dfs(n, row + 1, ans, res);
                ans[row][col] = '.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        auto res = vector<vector<string>>();
        auto ans = vector<string>(n,string(n,'.'));
        dfs(n,0,ans,res);
        return res;
    }
};
```

```C++ 
// 第二种
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        auto res = vector<vector<string>>();  // 答案数组
        auto ans = vector<string>(n, string(n,'.'));  //  n个皇后的位置
        auto cls = unordered_set<int>();  //  已经存放皇后的列
        auto diagonalRL = unordered_set<int>();  //  已经存放皇后的对角线 从左上到右下 同一对角线上行-列的值相等
        auto diagonalLR = unordered_set<int>();  //  已经存放皇后的对角线 从右上到左下 同一对角线上行+列的值相等
        backtrack(res, ans,cls, diagonalRL, diagonalLR, n, 0);
        return ans;
    }

    void backtrack(vector<vector<string>>& res, vector<string>& ans, unordered_set<int>& cls,
    unordered_set<int>& diagonalRL, unordered_set<int>& diagonalLR, int n, int row) {
        if (n == row) {  // 皇后的位置摆放完毕，保存当前解法
            res.push_back(ans);
            return;
        }
        //  如果没有摆放完毕
        for (int i = 0; i < n; i++) {  //  循环列
            int dr = row - i;
            int dl = row + i;
            if(cls.find(i) != cls.end() ||
            diagonalRL.find(dr) != diagonalRL.end() ||
            diagonalLR.find(dl) != diagonalLR.end()) {
                continue;  //  第row列i行会被其他皇后攻击到
            }
            cls.insert(i);
            diagonalRL.insert(dr);
            diagonalLR.insert(dl);
            ans[row][i] = 'Q';
            backtrack(ans, queens, cls, diagonalRL, diagonalLR, n, row + 1);
            //  回溯
            cls.erase(i);
            diagonalRL.erase(dr);
            diagonalLR.erase(dl);
            ans[row][i] = '.';
        }
    }
};
```

## 回溯算法

我个人认为回溯算法就是在深度优先遍历的同时检测是否符合条件。回溯算法和深度优先算法不同的是回溯算法强调当前遍历的状态是否符合条件，而深度优先算法则更加偏重于遍历所有状态。也有人说回溯算法像决策树，在不满足条件时直接剪枝。不过我觉得思路都差不多，都是在遍历全部状态的时候去掉不满足条件的状态，以此节约资源。

在看完这篇算法之后不知你能否了解到回溯算法的大概思路呢？欢迎讨论和提出您宝贵的意见。
