#### <a href="https://leetcode.cn/problems/maximize-score-after-n-operations/">N次操作后的最大分数和</a>

------------

```java
class Solution {
    public int maxScore(int[] nums) {
        int n = nums.length;
        int[] f = new int[1 << n];
        for (int i = 0; i < 1 << n; i ++) {
            // 当前是第几次操作
            int cnt = 0;
            for (int j = 0; j < n; j ++) {
                if ((i >> j & 1) == 0) cnt ++;
            }
            cnt = cnt / 2 + 1;

            // 枚举最大值
            for (int j = 0; j < n; j ++) {
                if ((i >> j & 1) == 1) {
                    for (int k = j + 1; k < n; k ++) {
                        if ((i >> k & 1) == 1) {
                            f[i] = Math.max(f[i], f[i - (1 << j) - (1 << k)] + gcd(nums[j], nums[k]) * cnt);
                        }
                    }
                }
            }
        }
        return f[(1 << n) - 1];
    }

    public int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

