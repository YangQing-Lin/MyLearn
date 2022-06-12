#### <a href="https://leetcode.cn/problems/sqrtx/submissions/">x 的平方根</a>

-----------

###### c++

```c++
class Solution {
public:
    int mySqrt(int x) {
        int l = 0, r = x;
        int res = -1;
        while (l <= r) {
            int mid = l + r >> 1;
            if ((long long)mid * mid <= x) {
                res = mid;
                l = mid + 1;
            }
            else {
                r = mid - 1;
            }
        }

        return res;
    }
};
```

###### java

```java
class Solution {
    public int mySqrt(int x) {
        int l = 0, r = x;
        int res = -1;
        while (l <= r) {
            int mid = l + r >> 1;
            if ((long)mid * mid <= x) {
                res = mid;
                l = mid + 1;
            }
            else {
                r = mid - 1;
            }
        }

        return res;
    }
}
```

