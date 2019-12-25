## Partition Equal Subset Sum(Medium)

### Problem Statement

Given a **non-empty** array containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Note:**

1. Each of the array element will not exceed 100.
2. The array size will not exceed 200.

 

**Example 1:**

```
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

 

**Example 2:**

```
Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```

### Solution

这个题是0这个题可以转换成求一个数组是否存在一个和为sum/2的子数组。实际上是一个0/1背包问题。

```
我们假设dp[i][j]表示从第一个到第i个数，是否能组成和为j的子数组。
则有：
选择第i个数字：dp[i][j] = dp[i-1][j-nums[i]]
不选择第i个数字： dp[i][j] = dp[i-1][j]
因此dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i]]
```

因此有如下代码：

```java
class Solution {
    public boolean canPartition(int[] nums) {
        if (nums.length < 2) {
            return false;
        }
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        if ((sum & 1) == 1) {
            return false;
        }
        sum /= 2;
        boolean[][] dp = new boolean[nums.length + 1][sum + 1];
        dp[0][0] = true;
        for (int i = 1; i <= nums.length; i++) {
            for (int j = 1; j <= sum; j++) {
                dp[i][j] = dp[i-1][j];
                if (j >= nums[i-1]) {
                    dp[i][j] = (dp[i][j] || dp[i-1][j-nums[i-1]]);
                }
            }
        }
        return dp[nums.length][sum];
    }
}
```

时间复杂度为O(nm)，空间复杂度是O(nm)

### Optimization

时间上没什么可以优化的了，但是空间上可以优化一下：

```java
public boolean canPartition(int[] nums) {
    int sum = 0;
    
    for (int num : nums) {
        sum += num;
    }
    
    if ((sum & 1) == 1) {
        return false;
    }
    sum /= 2;
    
    int n = nums.length;
    boolean[] dp = new boolean[sum+1];
    dp[0] = true;
    
    for (int num : nums) {
        for (int i = sum; i >= num; i--) {
            dp[i] = dp[i] || dp[i-num];
        }
    }
    
    return dp[sum];
}
```

此时的思路是这样的：

```
dp[i]表示子数组的和是否能为i，因此结果就是dp[sum/2]
dp[i] = dp[i] || dp[i-num]
```
此时空间复杂度为O(n)

### Follow up

判断是否能将一个数组分成k个和相等的子数组。即[698. Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

这个题就不能用动态规划了，而要用回溯。

```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        if (k <= 0 || sum % k != 0) {
            return false;
        }
        int[] visited = new int[nums.length];
        return help(nums, visited, 0, k, 0, 0, sum / k);
    }
    
    private boolean help(int[] nums, int[] visited, int index, int k, int cur_sum, int cur_count, int target) {
        if (k == 1) {
            return true;
        }
        if (cur_sum == target && cur_count > 0) {
            return help(nums, visited, 0, k - 1, 0, 0, target);
        }
        for (int i = index; i < nums.length; i++) {
            if (visited[i] == 1) {
                continue;
            }
            visited[i] = 1;
            if (help(nums, visited, i + 1, k, cur_sum + nums[i], cur_count + 1, target)) {
                return true;
            }
            visited[i] = 0;
        }
        return false;
    }
}
```

### Others

在讨论区有人提到[518. Coin Change 2](https://leetcode.com/problems/coin-change-2/)题，518的硬币题和这个子数组和题类似，也是用背包法：

```
dp[i][j] 表示使用前i个硬币组成面额为j的方法次数
dp[i-1][j] 表示完全不用第i个硬币
dp[i][j-coins[i - 1]] 表示至少用一个第i个硬币，两者为互斥时间
```

代码如下

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length + 1][amount + 1];
        dp[0][0] = 1;
        for (int i = 1; i <= coins.length; i++) {
            dp[i][0] = 1;
            for (int j = 1; j <= amount; j++) {
                dp[i][j] = dp[i - 1][j] + (j >= coins[i - 1] ? dp[i][j - coins[i - 1]] : 0);
            }
        }
        return dp[coins.length][amount];
    }
}
```

同理，该题也能优化：

```java
public int change(int amount, int[] coins) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;
    for (int coin : coins) {
        for (int i = coin; i <= amount; i++) {
            dp[i] += dp[i-coin];
        }
    }
    return dp[amount];
}
```

这个题是完全背包问题