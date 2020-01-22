## 字符串题

字符串相关题目的一些收集。

### 1.Microsoft | OA 2019 | Min Moves to Obtain String Without 3 Identical Consecutive Letters

![](https://assets.leetcode.com/users/siojl13/image_1570305005.png)

[原题地址](https://leetcode.com/discuss/interview-question/398026/)

```c++
int solution(string s){
	int res = 0;
	for(int i = 0 ; i < s.length() ;){
		int next = i+1;
		while(next < s.length() && s[i] == s[next]) next++;
		res += (next - i)/3;
		i = next;
	}
	return res;
}

分析过程：
there are fixed pattern we need to consider:
3 consecutive : baaab , replace the middle a
4 consecutive : baaaab, replace the third a
5 consecutive : baaaaab, replace the third a

for more than 5, replace the third char and it can be reduced as the above three conditions
6 consecutive : baaaaaab --> baabaaab ->trans to baaab with 1 replacement -> trans to bb with 2 replancement
10 consecutive : baaaaaaaaaab --> baabaaaaaaab ->trans to baaaaaaab with 1 replacement -> trans to baaaab with 2 replacement -> trans to baab with 3 replacement, done.

therefore, we can see the rule, counter += (consecutive char number)/3
    
总而言之就是找规律，连续字符串/3就行了
```

