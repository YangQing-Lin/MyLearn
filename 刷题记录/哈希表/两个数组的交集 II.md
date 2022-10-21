#### <a href="https://leetcode.cn/problems/intersection-of-two-arrays-ii/">两个数组的交集 II</a>

-------------

##### 哈希表记录其中一个数组中的数字出现的次数

然后遍历另一个数组，找到相同的元素

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        int n = nums1.length, m = nums2.length;
        if (n > m) return intersect(nums2, nums1);

        Map<Integer, Integer> map = new HashMap<>();
        for (int t : nums1) {
            map.put(t, map.getOrDefault(t, 0) + 1);
        }

        List<Integer> list = new ArrayList<>();
        for (int t : nums2) {
            if (map.containsKey(t)) {
                list.add(t);
                map.put(t, map.get(t) - 1);
                if (map.get(t) == 0) map.remove(t);
            }
        }

        int[] res = new int[list.size()];
        for (int i = 0; i < list.size(); i ++) {
            res[i] = list.get(i);
        }
        return res;
    }
}
```

