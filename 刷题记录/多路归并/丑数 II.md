#### <a href="https://leetcode.cn/problems/ugly-number-ii/">丑数 II</a>

-----------------

##### 多路归并算法：

假设 S 序列为长度为 n 的丑数序列

由丑数的定义可知，S序列可以由三部分组成：

- 1 * 2，2 * 2，3 * 2，4 * 2...由因数 2 得到的丑数序列
- 1 * 3，2 * 3，3 * 3，4 * 3...由因数 3 得到的丑数序列
- 1 * 5，2 * 5，3 * 5，4 * 5...由因数 5 得到的丑数序列

则定义三个指针分别指向三个序列，每次比较三个指针指到的值最小是多少，并把该最小值加到答案里，同时该指针往后移动一位

直到丑数序列的长度为 n 停止

##### 在实际代码中：

要实现上述思路，需要构造三个序列，构造三个序列可以直接在 S 序列上构造

```java
class Solution {
    public int nthUglyNumber(int n) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        for (int i = 0, j = 0, k = 0; list.size() < n;) {
            int t = Math.min(list.get(i) * 2, Math.min(list.get(j) * 3, list.get(k) * 5));
            list.add(t);
            if (list.get(i) * 2 == t) i ++;
            if (list.get(j) * 3 == t) j ++;
            if (list.get(k) * 5 == t) k ++;
        }
        return list.get(n - 1);
    }
}
```

