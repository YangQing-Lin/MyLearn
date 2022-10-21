#### <a href="https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/">两数之和 II - 输入有序数组</a>

---------------

```java
class Solution {
    public int[] twoSum(int[] num, int target) {
        int n = num.length;
        int[] arr = new int[2];
        for (int i = 0, j = n - 1; i < j; i ++) {
            while (i < j && num[i] + num[j] > target) j --;
            if (num[i] + num[j] == target) {
                arr[0] = i + 1;
                arr[1] = j + 1;
                break;
            }
        }
        return arr;
    }
}
```

