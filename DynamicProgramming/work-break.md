## Word Break(Medium)

### Problem Statment

Given a **non-empty** string *s* and a dictionary *wordDict* containing a list of **non-empty** words, determine if *s* can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

**Example 1:**

```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

### Solution

这个题给出一个字符串以及一个dict，求问dict中的单词能否组成这个字符串。

dp解法思路：

```
dp[i] 代表字符串中的前i个字符能否用wordDict中的单词拼成，长度为length + 1。
dp[0]代表一个字符都没有的情况，此时不用选wordDict中的任意单词也能拼成，因此为true。
因此我们可以发现一个推导方程，如果dp[i] = dp[j] && dict.contains(s.substring(j, i))。
dp[i]对应s[i-1]
```

因此有如下解法：

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length() + 1];
        Set<String> set = new HashSet<>();
        for (String word : wordDict) {
            set.add(word);
        }
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && set.contains(s.substring(j, i))) {
                    dp[i] = true;
                }
            }
        }
        return dp[s.length()];
    }
}
```

### Follow up

有了以上的解法，我们再看[472. Concatenated Words](https://leetcode.com/problems/concatenated-words/)题，给出一个字符串数组，返回字符串数组中能被其他字符拼接成的字符串。

思路如下：

```
因为长度长的字符只会被长度短的字符拼接，因此我们可以先按照长度排序一下：

Arrays.sort(word, (s1, s2) -> Integer.compare(s1.length(), s2.length()))

在遍历每一个word之后，将当前word加入到dict中，从而形成每一个word只会被preWord拼接的效果
```

代码如下：

```java
class Solution {
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        Arrays.sort(words, (s1, s2) -> Integer.compare(s1.length(), s2.length()));
        Set<String> dict = new HashSet<>();
        List<String> res = new ArrayList<>();
        for (String word : words) {
            if (canBeBreak(word, dict)) {
                res.add(word);
            }
            dict.add(word);
        }
        return res;
    }
    
    private boolean canBeBreak(String s, Set<String> dict) {
        if (dict.isEmpty()) {
            return false;
        }
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && dict.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```

