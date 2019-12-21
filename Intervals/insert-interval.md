## Insert Interval(Hard)

### Problem Statement

Given a set of *non-overlapping* intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

**Example 1:**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

### Solution

这个区间题是将一个区间插入到一个已经**排好序**的non-overlapping的区间数组中。注意这个题目也强调了已经根据start排好序了。这个比较简单，将区间数组分成三块，第一块和第三块与输入区间没有交集，中间一块与输入区间有交集，单独处理一下就可以了。

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        LinkedList<int[]> resultList = new LinkedList<>();
        int i = 0;
        while(i < intervals.length && intervals[i][1] < newInterval[0]) {
            resultList.add(intervals[i++]);
        }
        while(i < intervals.length && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = Math.min(intervals[i][0], newInterval[0]);
            newInterval[1] = Math.max(intervals[i][1], newInterval[1]);
            i++;
        }
        resultList.add(newInterval);
        while (i < intervals.length) {
            resultList.add(intervals[i++]);
        }
        return resultList.toArray(new int[resultList.size()][2]);
    }
}
```

