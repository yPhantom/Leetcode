## Count Square Submatrices with All Ones(Medium)

### Problem Statement

Given a `m * n` matrix of ones and zeros, return how many **square** submatrices have all ones.

**Example 1:**

```
Input: matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
Output: 15
Explanation: 
There are 10 squares of side 1.
There are 4 squares of side 2.
There is  1 square of side 3.
Total number of squares = 10 + 4 + 1 = 15.
```

**Example 2:**

```
Input: matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
Output: 7
Explanation: 
There are 6 squares of side 1.  
There is 1 square of side 2. 
Total number of squares = 6 + 1 = 7.
```

**Constraints:**

- `1 <= arr.length <= 300`
- `1 <= arr[0].length <= 300`
- `0 <= arr[i][j] <= 1`

### Solution

这个题目出现于[Weekly Contest 165](https://leetcode.com/contest/weekly-contest-165)。题意让计算一个m*n的矩阵中，一共有多少个正方形。

> 判断题型很关键

如何判断这道题是考察我们什么算法呢？根据题意，边长为2的正方形是由4个边长为1的正方形组成的，如果我们熟练的话，从这里就可以判断出该题符合**动态规划总是求解子问题，并将子问题合并求得结果**的思想。

既然是动态规划，那么就要找到其转移方程，这一步往往比较困难，对于这种矩阵类型题目，一般考虑一个右下角或者左上角这样的点：

1. 我们已经右下角的点作为基准，dp[i]\[j]代表以右下角坐标为(i, j)的最大正方形的边长，之所以可以这么选是因为这样可以确立唯一的一个正方形。
2. 寻找转移方程。我们发现：

```
if A[i][j] == 1:
    dp[i+1][j+1] = min(dp[i][j+1], dp[i+1][j], dp[i][j])+1
else:
    dp[i+1][j+1] = 0
```

因此可以得到我们初步的解决方案。

### Code

比较直接的解决方案(直接引用DISCUSS中的[高赞回答](https://leetcode.com/problems/count-square-submatrices-with-all-ones/discuss/441312/Java-Simple-DP-solution))：

```java
class Solution {
    public int countSquares(int[][] matrix) {
        int n = matrix.length;
        int m = matrix[0].length;
        int[][] dp = new int[n][m];
        int count = 0;
        
        for(int i = 0; i < n; i++) if(matrix[i][0] == 1) dp[i][0] = 1;
        for(int j = 0; j < m; j++) if(matrix[0][j] == 1) dp[0][j] = 1;
        // dp[i][j] will tell you the max side of the square with bottom right corner at (i, j).
        for(int i = 1; i < n; i++){
            for(int j = 1; j < m; j++){
                if(matrix[i][j] == 1){
                    int min = 1 + Math.min(dp[i - 1][j], Math.min(dp[i][j - 1], dp[i - 1][j - 1]));
                    dp[i][j] = min;
                }
            }
        }
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++) count += dp[i][j];
        }
        return count;
    }
}
```

此时的时间复杂度为O(m*n)，空间复杂度为O(m\*n)。

### Optimization

上述代码存在优化空间，我们可以将O(m*n)的空间优化到O(2\*n)，仔细观察转移方程：

```
dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
```

实际上`dp[i][j]`和`dp[i][j-1]`可以当成一行的相邻两数，`dp[i-1][j]`和`dp[i-1][j-1]`可以当成一行的相邻两数。整个方程实际上只涉及到两行，因此可以写出如下代码：

```java
class Solution {
    public int countSquares(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        int[] pre = new int[n];
        int[] cur = new int[n];
        int count = 0;
        for (int i = 0; i < n; i++) {
            pre[i] = matrix[0][i];
            count += matrix[0][i];
        }
        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (j == 0) {
                    cur[j] = matrix[i][0];
                } else if (matrix[i][j] == 1) {
                    cur[j] = 1 + Math.min(cur[j - 1], Math.min(pre[j], pre[j - 1]));
                } else {
                    cur[j] = 0;
                }
                count += cur[j];
            }
            // 注意这里的交换
            int[] tmp = pre;
            pre = cur;
            cur = tmp;
        }
        return count;
    }
}
```

再进一步，O(2n)的空间复杂度可以优化到O(n)。这里dp[j]，dp[j-1]代表pre[j]，pre[j-1]。pre代表cur[j-1]

```java
class Solution {
    public int countSquares(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        int[] dp = new int[n];
        int count = 0;
        for (int i = 0; i < n; i++) {
            dp[i] = matrix[0][i];
            count += matrix[0][i];
        }
        int pre = 0;
        int res = 0;
        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (j == 0) {
                    res = matrix[i][0];
                } else if (matrix[i][j] == 1) {
                    res = 1 + Math.min(pre, Math.min(dp[j], dp[j - 1]));
                } else {
                    res = 0;
                }
                count += res;
                pre = dp[j];
                dp[j] = res;
            }
        }
        return count;
    }
}
```

**时间复杂度**

此时的最优解的时间复杂度是O(mn)

**空间复杂度**

通过优化，将空间复杂度从O(mn) -> O(2n) - > O(n)。其实，如果题目中允许In-place修改的话，空间复杂度为O(1)。