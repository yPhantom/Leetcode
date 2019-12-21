# Intervals

区间类题目第一步，先将输入的区间数组按照每个区间的start升序排序。

我们主要看这四道题：

* [56. Merge Intervals](merge-intervals.md)
* [57. Insert Interval](insert-interval.md)
* [986. Interval List Intersections](interval-list-intersections.md)
* [1288. Remove Covered Intervals](remove-covered-intervals.md)

第一题给定一个区间数组，要求将overlapping的区间合并。解法需要先按照start**排序**

第二题给定一个**排好序**的non-overlapping区间数组，要求将一个区间插入，并仍然non-overlapping

第三题给定两个区间数组，求两个区间数组的交集。解法仍然需要**排好序**。

第四题给定一个区间数组，要求删除掉被covered的区间，求剩下的区间个数。解法还是需要排好序。



因此对于区间类的题目，第一步要做的就是排好序，这一步在Java中，如果直接调用Arrays.sort，那么时间复杂度就已经达到了O(nlogn)了。

这类题目常常针对是否covered，或者是否overlapping来出题。形式无非就是：

* 输入是一个区间数组，在这个区间数组里面进行各种操作
* 输入是一个区间数组和一个区间，要求满足一定条件
* 输入是两个或者两个以上的区间数组，比如986题是求两个区间数组的交集，那如果是三个或者三个以上呢？

