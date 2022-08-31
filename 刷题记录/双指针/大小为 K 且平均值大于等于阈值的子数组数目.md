#### <a href="https://leetcode.cn/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/">大小为 K 且平均值大于等于阈值的子数组数目</a>

----------

```java
class Solution {
    public int numOfSubarrays(int[] arr, int k, int ts) {
        int res = 0;
        for (int i = 0, j = 0, sum = 0; i < arr.length; i ++) {
            sum += arr[i];
            if (i - j >= k) {
                sum -= arr[j ++];
            }
            if (i - j + 1 == k && sum >= k * ts) {
                res ++;
            }
        }
        return res;
    }
}
```

