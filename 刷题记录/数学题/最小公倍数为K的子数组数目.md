#### <a href="https://leetcode.cn/problems/number-of-subarrays-with-lcm-equal-to-k/">最小公倍数为K的子数组数目</a>

-----------

##### 两个数的最小公倍数为：两数之积 / 两数的最大公约数

然后暴力枚举每个子数组判断是否满足要求即可

注意：break

```java
class Solution {
    public int subarrayLCM(int[] nums, int k) {
        int res = 0;
        for (int i = 0; i < nums.length; i ++) {
            int x = nums[i];
            for (int j = i; j < nums.length; j ++) {
                x = x * nums[j] / gcd(x, nums[j]);
                if (x == k) res ++;
                else if (x > k) break;
            }
        }
        return res;
    }

    public int gcd(int a, int b) {
        return b != 0 ? gcd(b, a % b) : a;
    }
}
```

