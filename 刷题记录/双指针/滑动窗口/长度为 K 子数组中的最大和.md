#### <a href="https://leetcode.cn/problems/maximum-sum-of-distinct-subarrays-with-length-k/">长度为 K 子数组中的最大和</a>

---------------

```java
class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        int n = nums.length;
        long res = 0;
        long sum = 0;
        for (int i = 0, j = 0; i < n; i ++) {
            int t = nums[i];
            if (i - j + 1 > k) {
                set.remove(nums[j]);
                sum -= nums[j];
                j ++;
            }
            if (!set.contains(t)) {
                set.add(t);
                sum += t;
            }
            else {
                while (j < n && set.contains(t)) {
                    set.remove(nums[j]);
                    sum -= nums[j];
                    j ++;
                }
                set.add(t);
                sum += t;
            }
            
            if (set.size() == k) {
                res = Math.max(sum, res);
            }
        }
        return res;
    }
}
```

