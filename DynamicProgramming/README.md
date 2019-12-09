# Dynamic Programming

动态规划背后的基本思想非常简单。大致上，若要解一个给定问题，我们需要解其不同部分（即子问题），再合并子问题的解以得出原问题的解。 通常许多子问题非常相似，为此动态规划法试图仅仅解决每个子问题一次，从而减少计算量： 一旦某个给定子问题的解已经算出，则将其[记忆化](http://zh.wikipedia.org/w/index.php?title=记忆化&action=edit&redlink=1)存储，以便下次需要同一个子问题解之时直接查表。 这种做法在重复子问题的数目关于输入的规模呈[指数增长](http://zh.wikipedia.org/wiki/指數增長)时特别有用。

https://blog.csdn.net/eagle_or_snail/article/details/50987044

https://wdxtub.com/interview/14520597062776.htm
