## 回溯法(BackTracking)

什么叫回溯法，回溯就是返回，在解决问题的过程中，我们按照一定的条件去给出（或者说枚举出）答案，当答案满足要求的时候就存储下来，当不满足要求的时候就进行「回溯」，回到上一步尝试另一种答案。这，就是回溯法。

也就是说，回溯法实际上是一个类似枚举的搜索尝试过程，主要是在搜索尝试过程中寻找问题的解，当发现已不满足求解条件时，就「回溯」返回，尝试别的路径。

> 我们可以将满足回溯条件的某个状态的点称为「回溯点」



上面是基本的概念，这些概念就不多说了，总而言之，记住几个关键词，「尝试」「返回」

在实际题目中，回溯法常常用来解决组合，排序，子集等问题，包含深度优先搜索的策略在其中。

我做了：

```
22. Generate Parentheses
37. Sudoku Solver
39. Combination Sum
40. Combination Sum II
46. Permutations
47. Permutations II
51. N-Queens
52. N-Queens
```

这些题目，它们都有一个基本上很通用的模板

> 参考 [issac3的解法](https://leetcode.com/problems/combination-sum/discuss/16502/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partitioning))



### 一些经验之谈

`backtrack`回溯函数的参数定义。回溯函数的参数主要分为三块，第一块保存结果，一般是个list，用来存放所有满足要求解；第二块记录部分解，表示每一次尝试过程中得到的解，要记录下来；第三块记录限制条件，比如字符串长度呀，以及一个用来记录每个元素是否被访问过的数组等。。

这里呢，要记住一点，在回溯过程中，我们一定要用一个变量来记录这个部分解，当部分解满足限制条件的时候呢，就成了可行解。

如果题目提到重复的问题，在backtrack的for循环中需要注意去重的问题，可以用if过滤，也可以在一开始的时候就Arrays.sort。

于记录部分解而言，如果题目返回类型是，比如`List<String>`，那么一般就用`+`符号去连接字符串，记录部分解就用一个字符串就行了，如果是类似`List<List<Integer>>`，就要用一个list来存它，这个时候add之后要remove来还原状态，在保存结果到res中的时候要深拷贝。





当然，上述是DFS的策略，都是递归调用，我们可以使用迭代的方式，用一个LinkedLsit，LinkedList实现了Queue，通过BFS的方式来调。

主要有以下几点：

1. 递归中的先知条件也差不多可以作为迭代中的while的终止条件
2. 每次从队列取上一轮的部分解，然后BFS生成一堆新的部分解，append到尾后