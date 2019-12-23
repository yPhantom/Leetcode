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

