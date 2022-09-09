#### <a href="https://leetcode.cn/problems/string-to-integer-atoi/">字符串转换整数 ( atoi )</a>

----------------------------

##### 秦九韶算法

```java
class Solution {
    public int myAtoi(String s) {
        // 去掉前导0
        int k = 0;
        while (k < s.length() && s.charAt(k) == ' ') {
            k ++;
        }
        if (k == s.length()) {
            return 0;
        }

        // 判断是整数还是负数
        int minus = 1;
        if (s.charAt(k) == '-') {
            minus = -1;
            k ++;
        } else if (s.charAt(k) == '+') {
            k ++;
        }

        // 秦九韶算法
        long res = 0;
        while (k < s.length() && s.charAt(k) >= '0' && s.charAt(k) <= '9') {
            res = res * 10 + s.charAt(k) - '0';
            k ++;
            if (res > Integer.MAX_VALUE) {
                break;
            }
        }
        
        res *= minus;
        if (res > Integer.MAX_VALUE) {
            return Integer.MAX_VALUE;
        } else if (res < Integer.MIN_VALUE) {
            return Integer.MIN_VALUE;
        }
        return (int) res;
    }
}
```

后发现题目要求：

> 如果整数数超过 32 位有符号整数范围 [−231,  231 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −231 的整数应该被固定为 −231 ，大于 231 − 1 的整数应该被固定为 231 − 1 。

虽然未明确是否禁止使用long来存储答案，但是为了**严谨**，下面将long换成int写出题解

```java
class Solution {
    public int myAtoi(String s) {
        // 去掉前导0
        int k = 0;
        while (k < s.length() && s.charAt(k) == ' ') {
            k ++;
        }
        if (k == s.length()) {
            return 0;
        }

        // 判断是整数还是负数
        int minus = 1;
        if (s.charAt(k) == '-') {
            minus = -1;
            k ++;
        } else if (s.charAt(k) == '+') {
            k ++;
        }

        // 秦九韶算法
        int res = 0;
        while (k < s.length() && s.charAt(k) >= '0' && s.charAt(k) <= '9') {
            if (res > (Integer.MAX_VALUE - (s.charAt(k) - '0')) / 10) {
                if (minus < 0) {
                    return Integer.MIN_VALUE;
                }
                res = Integer.MAX_VALUE;
                break;
            }
            res = res * 10 + s.charAt(k) - '0';
            k ++;
        }

        return res *= minus;
    }
}
```

