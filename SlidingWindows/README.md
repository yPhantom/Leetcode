leetcode上有个总结的挺好的[通用算法模板](https://leetcode-cn.com/problems/minimum-window-substring/solution/hua-dong-chuang-kou-suan-fa-tong-yong-si-xiang-by-/)

通用的算法思想：

```java
int left = 0; right = 0;

while (right < s.size()) {
    window.add(s[right++]);
    
    while(valid) {
        windows.remove(s[left++]);
    }
}
```

