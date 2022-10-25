#### <a href="https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/">两数之和 II</a>

----------------

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int n = numbers.length;
        for (int i = 0; i < n; i ++) {
            int l = i + 1, r = n - 1;
            while (l < r) {
                int mid = l + (r - l) / 2;
                if (numbers[mid] + numbers[i] >= target) r = mid;
                else l = mid + 1;
            }
            if (numbers[i] + numbers[r] == target) return new int[]{i + 1, r + 1};
        }
        return new int[0];
    }
}
```

