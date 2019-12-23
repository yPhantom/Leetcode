## Split Array Largest Sum(Hard)

### Probleam Statement

Given an array which consists of non-negative integers and an integer *m*, you can split the array into *m* non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these *m* subarrays.

**Note:**
If *n* is the length of array, assume the following constraints are satisfied:

- 1 ≤ *n* ≤ 1000
- 1 ≤ *m* ≤ min(50, *n*)

**Examples:**

```
Input:
nums = [7,2,5,10,8]
m = 2

Output:
18

Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
```

### Solution

这道题和[回文字符串分割](palindrome-partitioning-iii)的题目很像，都是将整个数组分成指定份数，并满足一定条件。因此我的解决思路如下：

1. 用dp[i]\[m]表示从下标0到下标i的数组分成m个子集的最小化的子集最大值。分成m个子集实际上就切m - 1刀，所以dp[i]\[0]就代表nums[0]到nums[i]的和。

2. 因为我们对于切割次数需要便利，从0一直遍历到m - 1，在相同切割次数下，我们需要取同一种切割方式的最大值中的最小值。因此当m指定时，我们要取最小值。

3. 因此转移方程可以写成：

```
dp[i][m] = min(dp[i][m], max(dp[j][m - 1], dp[i][0] - dp[j][0]))
```

因此正常dp的解法如下：

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        int n = nums.length;
        int[][] dp = new int[n][m];
        dp[0][0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            dp[i][0] = dp[i - 1][0] + nums[i]; 
        }
        for (int p = 1; p < m; p++) {
            for (int i = 0; i < nums.length; i++) {
                int min = Integer.MAX_VALUE;
                for (int j = 0; j < i; j++) {
                    min = Math.min(min, Math.max(dp[j][p - 1], dp[i][0] - dp[j][0]));
                }
                dp[i][p] = min;
            }
        }
        return dp[n - 1][m - 1];
    }
}
```

此时时间复杂度为O(m*n<sup>2</sup>)，空间复杂度为O(m\*n)

但是看了一下讨论区的方法，可以使用二分法的方式来做，真是学到了。

二分法的思路在于，如果我们切数组，那么数组的最大值为所有数的和sum，如果我们切的次数m等于n - 1，结果返回所有数中最大的一个max。因此呢，我们将数组分成m个子集，那么返回的结果应当在[max, sum]之间。

用一个例子来分析，nums = [1, 2, 3, 4, 5], m = 3，将 left 设为数组中的最大值5，right 设为数字之和 15，然后算出中间数为 10，接下来要做的是找出和最大且小于等于 10 的子数组的个数，[1, 2, 3, 4], [5]，可以看到无法分为3组，说明 mid 偏大，所以让 right=mid - 1，然后再次进行二分查找，算出 mid=7，再次找出和最大且小于等于7的子数组的个数，[1,2,3], [4], [5]，成功的找出了三组，说明 mid 还可以进一步降低，让 right=mid，再次进行二分查找，算出 mid=6，再次找出和最大且小于等于6的子数组的个数，[1,2,3], [4], [5]，成功的找出了三组，尝试着继续降低 mid，让 right=mid，再次进行二分查找，算出 mid=5，再次找出和最大且小于等于5的子数组的个数，[1,2], [3], [4], [5]，发现有4组，此时的 mid 太小了，应该增大 mid，让 left=mid+1，此时 left=6，right=5，循环退出了，返回 right 即可。

```java
public class Solution {
    public int splitArray(int[] nums, int m) {
        int max = 0; long sum = 0;
        for (int num : nums) {
            max = Math.max(num, max);
            sum += num;
        }
        if (m == 1) return (int)sum;
        //binary search
        long l = max; long r = sum;
        while (l <= r) {
            long mid = (l + r)/ 2;
            if (valid(mid, nums, m)) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        return (int)l;
    }
    public boolean valid(long target, int[] nums, int m) {
        int count = 1;
        long total = 0;
        for(int num : nums) {
            total += num;
            if (total > target) {
                total = num;
                count++;
                if (count > m) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

此时的时间复杂度O(nlogn)，空间复杂度O(n)