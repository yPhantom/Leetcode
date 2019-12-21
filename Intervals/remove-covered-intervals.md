## Remove Covered Intervals(Medium)

### Problem Statement

Given a list of intervals, remove all intervals that are covered by another interval in the list. Interval `[a,b)` is covered by interval `[c,d)` if and only if `c <= a` and `b <= d`.

After doing so, return the number of remaining intervals.

 

**Example 1:**

```
Input: intervals = [[1,4],[3,6],[2,8]]
Output: 2
Explanation: Interval [3,6] is covered by [2,8], therefore it is removed.
```

 

**Constraints:**

- `1 <= intervals.length <= 1000`
- `0 <= intervals[i][0] < intervals[i][1] <= 10^5`
- `intervals[i] != intervals[j]` for all `i != j`

### Solution

题意让删除掉所有covered的区间，返回最后剩下的区间个数。这里和[56. Merge Intervals](merge-intervals.md)类似，先按照start升序排好，用一个基准区间去一个个判断是否为convered即可。

```java
class Solution {
    public int removeCoveredIntervals(int[][] intervals) {
        Arrays.sort(intervals, (it1, it2) -> Integer.compare(it1[0], it2[0]));
        int[] newInterval = intervals[0];
        int res = intervals.length;
        for (int i = 1; i < intervals.length; i++) {
            if (newInterval[1] >= intervals[i][1]) {
                res--;
            } else if (newInterval[0] == intervals[i][0]) {
                res--;
            } else {
                newInterval = intervals[i];
            }
        }
        return res;
    }
}
```

