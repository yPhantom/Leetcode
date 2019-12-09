## Frog Jump(Hard)

类型：递推

### Problem Statement

A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positiossns (in units) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.

If the frog's last jump was *k* units, then its next jump must be either *k* - 1, *k*, or *k* + 1 units. Note that the frog can only jump in the forward direction.

**Note:**

- The number of stones is ≥ 2 and is < 1,100.
- Each stone's position will be a non-negative integer < 231.
- The first stone's position is always 0.

**Example 1:**

```
[0,1,3,5,6,8,12,17]

There are a total of 8 stones.
The first stone at the 0th unit, second stone at the 1st unit,
third stone at the 3rd unit, and so on...
The last stone at the 17th unit.

Return true. The frog can jump to the last stone by jumping 
1 unit to the 2nd stone, then 2 units to the 3rd stone, then 
2 units to the 4th stone, then 3 units to the 6th stone, 
4 units to the 7th stone, and 5 units to the 8th stone.
```

**Example 2:**

```
[0,1,2,3,4,8,9,11]

Return false. There is no way to jump to the last stone as 
the gap between the 5th and 6th stone is too large.
```

### Solution

403题青蛙过河，给定一个数组代表石头的位置，并指定每次跳跃的规则，返回青蛙能否跳到最后一块石头上。

题目有点绕，需要细细理解。这题的题意已经很明显的告诉了我们他们的递推关系，根据题意，假设青蛙到达了下标为i的石块，那么我们只要知道它的射程(下一次能跳多远)，就可以进行新一轮的判断了。因此主要问题在于如何知道每一块石头的射程。

这里不再是之前的动态规划里面的找找转移方程就好了，主要是一个递推的关系，从第一块石头一直递推到最后一块石头。

代码如下：

```java
class Solution {
    public boolean canCross(int[] stones) {
        HashMap<Integer, HashSet<Integer>> m = new HashMap<>();
        for (int i = 0; i < stones.length; i++) {
            m.put(stones[i], new HashSet<>());
        }
        m.get(0).add(1);
        for (int i = 0; i < stones.length - 1; i++) {
            int stone_value = stones[i];
            for (int step: m.get(stone_value)) {
                int reach = stone_value + step;
                if (reach == stones[stones.length - 1]) {
                    return true;
                }
                HashSet<Integer> s = m.get(reach);
                if (s != null) {
                    s.add(step + 1);
                    s.add(step);
                    if (step - 1 > 0) {
                        s.add(step - 1);
                    }
                }
            }
        }
        return false;
    }
}
```

**时间复杂度**

O(n) - O(n<sup>2</sup>)   # 原谅我不知道怎么算:cold_sweat:

**空间复杂度**

O(n) - O(n<sup>2</sup>)