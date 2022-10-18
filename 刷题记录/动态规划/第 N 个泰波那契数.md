#### <a href="https://leetcode.cn/problems/n-th-tribonacci-number/">第 N 个泰波那契数</a>

----------

```java
class Solution {
    public int tribonacci(int n) {
        if (n <= 2) return n < 2 ? n : 1;

        int t1 = 0, t2 = 0, t3 = 1, res = 1;
        for (int i = 3; i <= n; i ++) {
            t1 = t2;
            t2 = t3;
            t3 = res;
            res = t1 + t2 + t3;
        }
        return res;
    }
}
```

