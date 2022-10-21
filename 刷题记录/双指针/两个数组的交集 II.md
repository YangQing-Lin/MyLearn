#### <a href="https://leetcode.cn/problems/intersection-of-two-arrays-ii/">两个数组的交集 II</a>

-------------

##### 每次判断两个指针对应的值：

- 如果两个值相等，加到答案里，两个指针都向后移动
- 如果两个值不相等，将较小的值对应的指针向后移动

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        int n = nums1.length, m = nums2.length;
        Arrays.sort(nums1); Arrays.sort(nums2);
        List<Integer> list = new ArrayList<>();
        int i = 0, j = 0;
        while (i < n && j < m) {
            if (nums1[i] < nums2[j]) {
                i ++;
            } else if (nums1[i] > nums2[j]) {
                j ++;
            } else {
                list.add(nums2[j]);
                i ++; j ++;
            }
        }

        int[] res = new int[list.size()];
        for (int p = 0; p < list.size(); p ++) {
            res[p] = list.get(p);
        }
        return res;
    }
}
```

