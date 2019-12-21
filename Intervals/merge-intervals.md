## Merge Intervals(Medium)

### Probleam Statement

Given a collection of intervals, merge all overlapping intervals.

**Example 1:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**

```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

### Solution

这个题目其实不难，面对区间题目比较关键的是要将这些区间以start进行排序，这样会简单很多。我们选定第一个区间作为基准，然后遍历整个区间数组，先将基准区间加入到存放结果的list中，如果两个区间能够合并，则更新结果list中的基准数组的右边界。如果不能合并，则替换掉基准区间。

上述过程的实质就是用基准区间去比较、合并，基准区间全部来自于输入的区间数组，因此完成了合并。

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length <= 1) {
            return intervals;
        }
        Arrays.sort(intervals, (it1, it2) -> Integer.compare(it1[0], it2[0]));
        List<int[]> res = new ArrayList<>();
        int[] travelIt = intervals[0];
        res.add(travelIt);
        for (int[] interval: intervals) {
            if (interval[0] <= travelIt[1]) {
                travelIt[1] = Math.max(interval[1], travelIt[1]);
            } else {
                travelIt = interval;
                res.add(travelIt);
            }
        }
        return res.toArray(new int[res.size()][]);
    }
}
```

因为Java的Arrays.sort函数的时间复杂度是O(nlogn)，因此总的时间复杂度就是O(nlogn)，空间复杂度是O(2n)