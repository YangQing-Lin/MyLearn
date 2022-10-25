#### <a href="https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/">买卖股票的最佳时机 II</a>

-------------

##### 参考题解：[官方](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/solution/mai-mai-gu-piao-de-zui-jia-shi-ji-ii-by-leetcode-s/)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] f = new int[n][2];
        f[0][0] = 0; f[0][1] = -prices[0];
        for (int i = 1; i < n; i ++) {
            f[i][0] = Math.max(f[i - 1][0], f[i - 1][1] + prices[i]);
            f[i][1] = Math.max(f[i - 1][1], f[i - 1][0] - prices[i]);
        }
        return f[n - 1][0];
    }
}
```

##### 滚动数组优化

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int f0 = 0, f1 = -prices[0];
        for (int i = 1; i < prices.length; i ++) {
            f0 = Math.max(f0, f1 + prices[i]);
            f1 = Math.max(f1, f0 - prices[i]);
        }
        return f0;
    }
}
```

