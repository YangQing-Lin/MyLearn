#### <a href="https://leetcode.cn/problems/minimize-xor/">最小 XOR</a>

---------

根据题目给定数据范围可知：最大的位数是30位（2<sup>0</sup> ~ 2<sup>29</sup>）

##### 思路：

1. 先算出 x 的置位数（num2 的置位数）

2. ##### 贪心思想：

   对于 num1 的每一位 p

   - 如果 p 为 1，x 的 p 位是 1，异或之后值变小
   - 如果 p 为 0，x 的 p 位是 1，异或之后值变大

   且**如果 p 位数越高，值变小或者变大的程度越大**

   所以**要使 num1 的值与 x 异或之后最小（减去的越多，加上的越小）**，则设 num1 的偏移量为 res

   - 从高到低枚举每一位，如果 p 为 1，则 res 减去这一位 1
   - 从低到高枚举每一位，如果 p 为 0，则 res 加上这一位 1

3. 最后 num1 的最小值为 （num1 + res），**又因为 x ^ num1 = min，则 x = min ^ num1** 

##### 题解：

```java
class Solution {
    public int minimizeXor(int a, int b) {
        // x 的置位数
        int cnt = 0;
        while (b > 0) {
            cnt += b & 1;
            b >>= 1;
        }

        // 根据数据范围可知，该值二进制表示中最多有30位
        int res = 0;
        for (int i = 29; i >= 0 && cnt > 0; i --) {
            if ((a >> i & 1) == 1) {
                res -= 1 << i;
                cnt --;
            }
        }
        for (int i = 0; i < 30 && cnt > 0; i ++) {
            if ((a >> i & 1) != 1) {
                res += 1 << i;
                cnt --;
            }
        }
        return (res + a) ^ a;
    }
}
```

