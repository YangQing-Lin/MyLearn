#### <a href="https://leetcode.cn/problems/number-of-ways-to-reach-a-position-after-exactly-k-steps/">恰好移动 k 步到达某一位置的方法数目</a>

--------------

```java
class Solution {

    int N = 2010, MOD = (int) 1e9 + 7;
    int[][] f = new int[1010][N]; // i 表示步数，j 表示下标

    public int numberOfWays(int startPos, int endPos, int k) {
        startPos += 500;
        endPos += 500;
        f[0][startPos] = 1;
        for (int i = 1; i <= k; i ++) {
            for (int j = 0; j < N; j ++) {
                if (j > 0) {
                    f[i][j] = f[i - 1][j - 1];
                }
                if (j + 1 < N) {
                    f[i][j] = (f[i][j] + f[i - 1][j + 1]) % MOD;
                }
            }
        }
        return f[k][endPos];
    }
}
```

