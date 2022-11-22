#### <a href="https://leetcode.cn/problems/nth-magical-number/">第 N 个神奇数字</a>

------------

##### 参考题解：[官方](https://leetcode.cn/problems/nth-magical-number/solution/di-n-ge-shen-qi-shu-zi-by-leetcode-solut-6vyy/)

每次得到范围内的神奇个数数量，如果数量恰好等于 $n$，则范围右端点的最小值就是要找的第 $n$ 个神奇数字

```java
class Solution {
    public int nthMagicalNumber(int n, int a, int b) {
        int MOD = (int) 1e9 + 7;
        long left = 1, right = (long) 4e13;
        while (left < right) {
            long mid = left + (right - left) / 2;
            if (get(mid, a, b) >= n) right = mid;
            else left = mid + 1;
        }
        return (int) (right % MOD);
    }

    // 求最大公约数，用于求最小公倍数
    public int gcb(int a, int b) {
        return b == 0 ? a : gcb(b, a % b);
    }

    // 求范围为 [1, x] 内的神奇数字的数量
    public long get(long x, int a, int b) {
        return x / a + x / b - x / ((long) a * b / gcb(a, b));
    }
}
```

