#### <a href="https://leetcode.cn/problems/zigzag-conversion/">Z 字形变换</a>

--------------

##### 找规律

公差为 $t = 2 * n - 2$，一共有 i = [0, n - 1] 列

第一行和最后一行的序列都是以 i 数字为开头，公差为 t 的等差数列

中间几行都是两个等差数列交叉排列，且均是以数字 t - i 为开头，t 为公差的等差数列

##### 题解

```java
class Solution {
    public String convert(String s, int n) {
        if (n == 1) {
            return s;
        }

        StringBuilder sb = new StringBuilder();
        int t = 2 * n - 2; // 等差数列的公差
        for (int i = 0; i < n; i ++) {
            if (i == 0 || i == n - 1) {
                for (int j = i; j < s.length(); j += t) {
                    sb.append(s.charAt(j));
                }
            } else {
                for (int j = i, k = t - i; j < s.length() || k < s.length(); j += t, k += t) {
                    if (j < s.length()) {
                        sb.append(s.charAt(j));
                    }
                    if (k < s.length()) {
                        sb.append(s.charAt(k));
                    }
                }
            }
        }
        return sb.toString();
    }
}
```

