## 并查集题目分析

先写个UF类，然后联合+查找就完事了，

例题：

- [128.Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence)
- [130.Surrounded Regions](https://leetcode.com/problems/surrounded-regions)
- [200.Number of Islands](https://leetcode.com/problems/number-of-islands) 
- [399.Evaluate Division](https://leetcode.com/problems/evaluate-division)
- [547.Friend Circles](https://leetcode.com/problems/friend-circles)
- [1319.Number of Operations to Make Network Connected](https://leetcode.com/problems/number-of-operations-to-make-network-connected) 

---

并查集即Union Find，联合与查找，常用来解决连通性问题。主要有两个过程：

- 联合，按照题意将两个对象放在一棵树上
- 查找，根据组别查找，即可得到题目所求。

并查集题目的关键字：**传递性**，**连通性**。比如互为朋友，网络节点的连接，乘除法的传递等等。

代码大致有以下流程：

### 1.初始化

申请一个大小为n的数组作为`parents`，并将`parents`中的每一个值都初始化为对应的下标`i`。为什么这么做呢？我们一开始将每个数都当作是属于自己的一个组别/分类/树，因此直接用下标`i`来表示所属的组，当然我们也可以用其他的来表示，但是这种利用下标的方式比较简单直接。有如下代码：

```java
class UF() {
    private int[] parents;

    public UF(int n) {
        parents = new int[n];
        for (int i = 0; i < n; i++) {
            parents[i] = i;
        }
    }
}
```

直接定义一个UF类的构造函数。

有些题目并不能用整形来作为类别，我们可以用HashMap。此时我们就不需要构造函数了，只需要在每次查找过程中先进行一个默认初始化即可，代码如下：

```java
class UF() {
    private Map<String, String> parents = new HashMap<>();

    public String find(String s) {
        if (!parents.containsKey(s)) {
            parents.put(s, s);
        }
    }
}
```

### 2.联合

联合比较简单，根据题意来将两个节点归属到一个组别/分类/树中。也就是将`parents`中对应的值改成一个。首先找到你要联合的两个节点的最顶端的节点，如果这两个节点不是同一类，那么让它俩归属同一类。这里的`parents[root1] = root2`或者`parents[root2] = root1`都可以。

```java
public void union(int node1, int node2) {
    // find函数见第三点/
    int root1 = find(node1);
    int root2 = find(node2);
    if (root1 != root2) {
        parents[root1] = root2;
    }
}
```

上述是`Quick Union`，不quick的union如下，加入了原本在find中的路径压缩。

### 3.查找

`Quick Union` + 一般查找：

```java
public int find(int node) {
    while (node != parents[node]) {
        parents[node] = parents[parents[node]];
        node = parents[node];
    }
    return node;
}
```

这里其实就是往上找到根节点，然后更改根节点所属的组。

普通的union + `Quick Find`：

```java
public int find(int node) {
    return parents[node];
}

public void union(int node1, int node2) {
    int root1 = find(node1);
    int root2 = find(node2);
    if (root1 == root2) {
        return;
    }
    for (int i = 0; i < parents.length; i++) {
        if (parents[i] == root1) {
            parents[i] = root2;
        }
    }
}
```

### 4.代码模板：

一维/二维数组（见[Leetcode 130题](https://leetcode.com/problems/surrounded-regions/description/)）：

```java
class UF {

    private int[] parents;

    public UF(int n) {
        parents = new int[n];
        for (int i = 0; i < n; i++) {
            parents[i] = i;
        }
    }

    private int find (int node) {
        while(node != parents[node]) {
            parents[node] = parents[parents[node]];
            node = parents[node];
        }
        return node;
    }

    private void union(int node1, int node2) {
        int root1 = find(node1);
        int root2 = find(node2);
        if (root1 != root2) {
            parents[root1] = root2;
        }
    }

    private boolean isConnected(int node1, int node2) {
        return find(node1) == find(node2);
    }
}
```