#### <a href="https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/">在排序数组中查找数字 I</a>

----------------

```java
class Solution {
    public int search(int[] nums, int target) {
        int res = 0;
        for (int num : nums) {
            if (num == target) res++;
        }
        return res;
    }
}
```

