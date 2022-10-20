#### <a href="https://leetcode.cn/problems/k-th-symbol-in-grammar/">第K个语法符号</a>

-----------

##### 倒推 + 奇偶校验

递推得到第 n 层第 k 个数字是从上一层中哪一个数字产生的

然后根据这个数字是 1 或 0，就可以对k进行奇偶校验

```java
class Solution {
    public int kthGrammar(int n, int k) {
        if (n == 1) return 0;
        int res = kthGrammar(n - 1, (k + 1) / 2);
        if (res == 0) return k % 2 == 1 ? 0 : 1;
        else return k % 2 == 1 ? 1 : 0;
    }
}
```

