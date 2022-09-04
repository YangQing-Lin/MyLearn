#### <a href="https://leetcode.cn/problems/number-of-ways-to-reach-a-position-after-exactly-k-steps/">恰好移动 k 步到达某一位置的方法数目</a>

-----------

##### 组合数，求逆元

```java
class Solution {

    int N = 1010;
    int MOD = (int) 1e9 + 7;
    long[] fact = new long[N]; // i的阶层
    long[] infact = new long[N]; // i的阶层的逆元

    public int numberOfWays(int sp, int ep, int k) {
        int m = ep - sp;
        if ((k - m) % 2 != 0 || k < m) {
            return 0;
        }

        int r = (m + k) / 2;

        init();

        return (int) (fact[k] * infact[r] % MOD * infact[k - r] % MOD);
    }

    public void init() {
        fact[0] = infact[0] = 1;
        for (int i = 1; i < N; i ++) {
            fact[i] = fact[i - 1] * i % MOD;
            infact[i] = infact[i - 1] * qmi(i, MOD - 2, MOD) % MOD;
        }
    }

    public long qmi(long a, int k, int p) {
        long res = 1;
        while (k != 0) {
            if ((k & 1) == 1) {
                res = res * a % p;
            }
            a = a * a % p;
            k >>= 1;
        }
        return res;
    }
}
```

