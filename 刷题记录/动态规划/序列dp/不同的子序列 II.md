#### <a href="https://leetcode.cn/problems/distinct-subsequences-ii/">不同的子序列 II</a>

------------

##### 参考题解：[宫水三叶](https://leetcode.cn/problems/distinct-subsequences-ii/solution/by-ac_oier-ph94/)

```java
class Solution {
    public int distinctSubseqII(String s) {
        int n = s.length(), MOD = (int) 1e9 + 7;
        int[][] f = new int[n + 1][26];
        for (int i = 1; i <= n; i ++) {
            int c = s.charAt(i - 1) - 'a';
            for (int j = 0; j < 26; j ++) {
                if (c != j) {
                    f[i][j] = f[i - 1][j];
                } else {
                    int cur = 1;
                    for (int k = 0; k < 26; k ++) cur = (cur + f[i -1][k]) % MOD;
                    f[i][j] = cur;
                }
            }
        }

        int res = 0;
        for (int i = 0; i < 26; i ++) res = (res + f[n][i]) % MOD;
        return res;
    }
}
```

