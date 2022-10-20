#### <a href="https://leetcode.cn/problems/house-robber-ii/">打家劫舍 II</a>

---------------

##### 如果房屋的数量不小于2，那么就将第一个房屋和最后一个房屋分开考虑

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        if (n == 2) return Math.max(nums[0], nums[1]);
        return Math.max(help(nums, 0, n - 2), help(nums, 1, n - 1));
    }

    public int help(int[] nums, int l, int r) {
        int first = nums[l], second = Math.max(nums[l], nums[l + 1]);
        for (int i = l + 2; i <= r; i ++) {
            int temp = second;
            second = Math.max(first + nums[i], second);
            first = temp;
        }
        return second;
    }
}
```

