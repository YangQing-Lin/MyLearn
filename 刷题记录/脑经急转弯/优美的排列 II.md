#### <a href="https://leetcode.cn/problems/beautiful-arrangement-ii/">优美的排列 II</a>

------------

##### 思路

当 k=1 时，我们将 1∼n 按照 [1,2,⋯,n] 的顺序进行排列，那么相邻的差均为 1，满足 k=1 的要求。

当 k=n-1 时，我们将 1∼n 按照 [1,n,2,n−1,3,⋯] 的顺序进行排列，那么相邻的差从 n-1 开始，依次递减 1。这样一来，所有从 1 到 n-1 的差值均出现一次，满足 k=n-1 的要求。

对于其它的一般情况，我们可以将这两种特殊情况进行合并，即列表的前半部分相邻差均为 1，后半部分相邻差从 k 开始逐渐递减到 1，这样从 1 到 k 的差值均出现一次，对应的列表即为：

$$
[1,2,⋯,n−k,n,n−k+1,n−1,n−k+2,⋯]
$$

##### 代码实现

```java
class Solution {
    public int[] constructArray(int n, int k) {
        List<Integer> list = new ArrayList<>();
        for (int i = 1; i < n - k; i ++) {
            list.add(i);
        }

        // 从 n 递减到 n-k
        for (int i = n - k, j = n; i <= j; i ++, j --) {
            list.add(i);
            if (i != j) {
                list.add(j);
            }
        }

        int[] res = new int[n];
        for (int i = 0; i < list.size(); i ++) {
            res[i] = list.get(i);
        }
        return res;
    }
}
```

