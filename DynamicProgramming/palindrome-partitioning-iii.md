## Palindrome Partitioning III(Hard)

### Probleam Statement

You are given a string `s` containing lowercase letters and an integer `k`. You need to :

- First, change some characters of `s` to other lowercase English letters.
- Then divide `s` into `k` non-empty disjoint substrings such that each substring is palindrome.

Return the minimal number of characters that you need to change to divide the string.

**Example 1:**

```
Input: s = "abc", k = 2
Output: 1
Explanation: You can split the string into "ab" and "c", and change 1 character in "ab" to make it palindrome.
```

**Example 2:**

```
Input: s = "aabbc", k = 3
Output: 0
Explanation: You can split the string into "aa", "bb" and "c", all of them are palindrome.
```

**Example 3:**

```
Input: s = "leetcode", k = 8
Output: 0
```

**Constraints:**

- `1 <= k <= s.length <= 100`.
- `s` only contains lowercase English letters.

### Solution

这个题目出现于[Weekly Contest 165](https://leetcode.com/contest/weekly-contest-165)的第四题，作为每周content的压轴题，难度确实足够。题意让替换一个字符串任意个数的字符，并将其切割k次，使得每个子字符串都是回文字符串。求最少需要替换多少个字符。

题目有两点：

* 字符串要是回文字符串
* 并且能切割k次后仍然回文

我们秉着收敛一个条件的原则，先计算求得让整个字符串都回文需要多少次.

定义一个pd[i]\[j]，表示从字符串的下标i到下标j的字符串回文需要替换的字符个数。那么有三种情况：

* i == j，pd[i]\[j] = 0
* i == j - 1，pd[i]\[j] = s[i]和s[j]是否相等
* 其他情况
  * 如果s[i] == s[j]，pd[i]\[j] = pd[i+1]\[j-1]
  * 否则，pd[i]\[j] = pd[i+1]\[j-1] + 1

在已知让整个字符串回文需要的次数后，我们再来考虑这个问题可以发现，我们只需要再定义一个dp数组dp[j]\[k]，代表从字符串0 ~ j共切割k次需要的最小替换次数。则有：

* dp[j]\[k] = min(dp[m]\[k - 1] + pd[m + 1]\[j]，dp[j]\[k])

因此我们可以写出如下的代码：

```java
class Solution {
    public int palindromePartition(String s, int k) {
        int len = s.length();
        int[][] pd = new int[len + 1][len];
        int[][] dp = new int[len][k + 1];
        
        for (int j = 0; j < len; j++) {
            for (int i = 0; i <= j; i++) {
                if (i == j) {
                    pd[i][j] = 0;
                } else if (j - i == 1) {
                    pd[i][j] = s.charAt(j) == s.charAt(i) ? 0 : 1;
                } else {
                    pd[i][j] = s.charAt(j) == s.charAt(i) ? pd[i + 1][j - 1] : pd[i + 1][j - 1] + 1;
                }
            }
        }
        
        for (int j = 0; j < len; j++) {
            dp[j][1] = pd[0][j];
        }
        
        for (int p = 2; p <= k; p++) {
            for (int j = 1; j < len; j++) {
                dp[j][p] = j + 1;
                for (int l = 0; l <= j; l++) {
                    dp[j][p] = Math.min(dp[l][p - 1] + pd[l + 1][j], dp[j][p]);
                }
            }
        }
        return dp[len - 1][k];
    }
}
```

**时间复杂度**

O(n<sup>2</sup>k)

**空间复杂度**

O(2n<sup>2</sup>)