#### <a href="https://leetcode.cn/problems/get-kth-magic-number-lcci/">第 k 个数</a>

-----------

```java
class Solution {
    public int getKthMagicNumber(int k) {
        int[] f = new int[]{3, 5, 7};
        HashSet<Long> set = new HashSet<>();
        PriorityQueue<Long> priorityQueue = new PriorityQueue<>();
        set.add(1l);
        priorityQueue.add(1l);
        int res = 0;
        for (int i = 0; i < k; i ++) {
            long cur = priorityQueue.poll();
            res = (int) cur;
            for (int t : f) {
                long a = cur * t;
                if (set.add(a)) {
                    priorityQueue.add(a);
                }
            }
        }
        return res;
    }
}
```

