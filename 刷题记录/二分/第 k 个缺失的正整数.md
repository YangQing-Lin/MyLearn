#### <a href="https://leetcode.cn/problems/kth-missing-positive-number/">第 k 个缺失的正整数</a>

-------------

##### 参考题解：[码友题解](https://leetcode.cn/problems/kth-missing-positive-number/solution/-by-lcfgrn-kh3k/)

```java
class Solution {
    public int findKthPositive(int[] arr, int k) {
        if (arr[0] > k) return k;

        int l = 0, r = arr.length;
        while (l < r) {
            int mid = l + (r - l) / 2;
            int p = arr[mid] - mid - 1; // 在当前下标时，缺失的元素个数
            if (p >= k) r = mid;
            else l = mid + 1;
        }
        return k - (arr[l - 1] - (l - 1) - 1) + arr[l - 1];
    }
}
```

