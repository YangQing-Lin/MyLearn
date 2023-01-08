#### <a href="https://leetcode.cn/problems/maximal-score-after-applying-k-operations/">执行 K 次操作后的最大分数</a>

-------------

```java
class Solution {
    public long maxKelements(int[] nums, int k) {
        PriorityQueue<Integer> heap = new PriorityQueue<>((o1, o2)->o2.compareTo(o1));
        for (int i = 0; i < nums.length; i ++) {
            heap.add(nums[i]);
        }

        long res = 0;
        while (k -- > 0) {
            int t = heap.poll();
            res += t;
            t = (t + 2) / 3;
            heap.add(t);
        }
        return res;
    }
}
```

