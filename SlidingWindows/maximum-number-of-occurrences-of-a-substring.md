##  Maximum Number of Occurrences of a Substring(Medium)

### Problem Statment

Given a string `s`, return the maximum number of ocurrences of **any** substring under the following rules:

- The number of unique characters in the substring must be less than or equal to `maxLetters`.
- The substring size must be between `minSize` and `maxSize` inclusive.

 

**Example 1:**

```
Input: s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
Output: 2
Explanation: Substring "aab" has 2 ocurrences in the original string.
It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).
```

**Example 2:**

```
Input: s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3
Output: 2
Explanation: Substring "aaa" occur 2 times in the string. It can overlap.
```

**Example 3:**

```
Input: s = "aabcabcab", maxLetters = 2, minSize = 2, maxSize = 3
Output: 3
```

**Example 4:**

```
Input: s = "abcde", maxLetters = 2, minSize = 3, maxSize = 3
Output: 0
```

 

**Constraints:**

- `1 <= s.length <= 10^5`
- `1 <= maxLetters <= 26`
- `1 <= minSize <= maxSize <= min(26, s.length)`
- `s` only contains lowercase English letters.

### Solution

这个题比较难的一点在于我们只需要考虑子字符串的长度为minSize就行了。为什么呢？如果字符串"abcd"出现次数最多，那么"abcd"的子字符串，比如"abc"出现次数必定大于等于"abcd"出现的次数。这是一种贪心greedy的思想。因此该题实际上是绕了一个弯的滑动窗口题。

普通版本


```java
class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        Map<String, Integer> count = new HashMap<>();
        Map<Character, Integer> occurrence = new HashMap<>();
        int res = 0;
        int start = 0;
        for (int i = 0; i < s.length(); i++) {
            char cur = s.charAt(i);
            occurrence.put(cur, occurrence.getOrDefault(cur, 0) + 1);
            if (i - start + 1 > minSize) {
                char c = s.charAt(start++);
                occurrence.put(c, occurrence.get(c) - 1);
                if (occurrence.get(c) == 0) {
                    occurrence.remove(c);
                }
            }
            if (i - start + 1 == minSize && occurrence.size() <= maxLetters) {
                String subStr = s.substring(start, i + 1);
                count.put(subStr, count.getOrDefault(subStr, 0) + 1);
                res = Math.max(res, count.get(subStr));
            }
            
        }
        return res;
    }
}
```

按照算法模板写了一下：

```java
class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        Map<String, Integer> count = new HashMap<>();
        Map<Character, Integer> occurrence = new HashMap<>();
        int res = 0;
        int left = 0;
        int right = 0;
        while(right < s.length()) {
            char cur = s.charAt(right++);
            occurrence.put(cur, occurrence.getOrDefault(cur, 0) + 1);
            while (right - left > minSize) {
                char c = s.charAt(left++);
                occurrence.put(c, occurrence.get(c) - 1);
                if (occurrence.get(c) == 0) {
                    occurrence.remove(c);
                }
            }
            if (right - left == minSize && occurrence.size() <= maxLetters) {
                String subStr = s.substring(left, right);
                count.put(subStr, count.getOrDefault(subStr, 0) + 1);
                res = Math.max(res, count.get(subStr));
            }
            
        }
        return res;
    }
}
```

实际上对于字符串用算法模板这种while循环比较方便，因为在一开始计算字符出现次数的时候就right++了，后续求substring的时候就不用right-left+1了。

**时间复杂度**

O(n<sup>2</sup>)