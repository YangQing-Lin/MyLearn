#### <a href="https://leetcode.cn/problems/contains-duplicate-ii/">存在重复元素 II</a>

-------------

##### 自己写的笨哈希表使用方法

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < nums.length; i ++) {
            if (!map.containsKey(nums[i])) {
                map.put(nums[i], new ArrayList<>());
            }
            map.get(nums[i]).add(i);
        }

        for (Map.Entry<Integer, List<Integer>> p : map.entrySet()) {
            if (p.getValue().size() > 1) {
                for (int i = 0; i < p.getValue().size(); i ++) {
                    for (int j = i + 1; j < p.getValue().size(); j ++) {
                        if (Math.abs(p.getValue().get(i) - p.getValue().get(j)) <= k) {
                            return true;
                        }
                    }
                }
            }
        }
        return false;
    }
}
```

##### 官方：

因为比较的是两个下标的差值最小

所以每次将最大的下标加入哈希表

如果已经存在，则将当前的下标和哈希表里的下标相比较，这两个下标距离此时是最近的

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i ++) {
            if (map.containsKey(nums[i]) && i - map.get(nums[i]) <= k) {
                return true;
            }
            map.put(nums[i], i);
        }
        return false;
    }
}
```

