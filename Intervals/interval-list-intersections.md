## Interval List Intersections(Medium)

### Problem Statement

Given two lists of **closed** intervals, each list of intervals is pairwise disjoint and in sorted order.

Return the intersection of these two interval lists.

*(Formally, a closed interval [a, b] (with a <= b) denotes the set of real numbers x with a <= x <= b.  The intersection of two closed intervals is a set of real numbers that is either empty, or can be represented as a closed interval.  For example, the intersection of [1, 3] and [2, 4] is [2, 3].)*

 

**Example 1:**

**![img](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)**

```
Input: A = [[0,2],[5,10],[13,23],[24,25]], B = [[1,5],[8,12],[15,24],[25,26]]
Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
Reminder: The inputs and the desired output are lists of Interval objects, and not arrays or lists.
```

 

**Note:**

1. `0 <= A.length < 1000`
2. `0 <= B.length < 1000`
3. `0 <= A[i].start, A[i].end, B[i].start, B[i].end < 10^9`

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

### Solution

这题是取两个区间数组的交集，一开始我用的是O(n<sup>2</sup>)的暴力遍历法。只打败了6%的对手......

原来这题可以用O(n+m)解决，双指针法，定义两个指针分别指着两个数组的"头"区间，每次交完之后的end较小的区间弃掉，指针后移。

```java
class Solution {
    public int[][] intervalIntersection(int[][] A, int[][] B) {
        List<int[]> res = new ArrayList<>();
        int i = 0;
        int j = 0;
        while(i < A.length && j < B.length) {
            int maxStart = Math.max(A[i][0], B[j][0]);
            int minEnd = Math.min(A[i][1], B[j][1]);
            
            if (maxStart <= minEnd) {
                res.add(new int[]{maxStart, minEnd});
            }
            
            if (A[i][1] == minEnd) {
                i++;
            }
            if (B[j][1] == minEnd) {
                j++;
            }
        }
        return res.toArray(new int[res.size()][2]);
    }
}
```

