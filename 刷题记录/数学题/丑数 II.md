#### <a href="https://leetcode.cn/problems/ugly-number-ii/">丑数 II</a>

---------------

##### 参考题解：[最小堆](https://leetcode.cn/problems/ugly-number-ii/solution/chou-shu-ii-by-leetcode-solution-uoqd/)

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] factors = new int[]{2, 3, 5};
        PriorityQueue<Long> heap = new PriorityQueue<>(); // 防止数据溢出，对最小值有影响
        heap.offer(1L);
        Set<Long> set = new HashSet<>();
        set.add(1L);
        int res = 0;
        while (n -- > 0) {
            long cur = heap.poll();
            res = (int) cur;
            for (int t : factors) {
                if (!set.contains(cur * t)) {
                    heap.offer(cur * t);
                    set.add(cur * t);
                }
            }
        }
        return res;
    }
}
```

