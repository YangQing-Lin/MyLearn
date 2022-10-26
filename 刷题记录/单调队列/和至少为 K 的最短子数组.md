#### <a href="https://leetcode.cn/problems/shortest-subarray-with-sum-at-least-k/">和至少为 K 的最短子数组</a>

-------------

##### 参考题解：[灵茶山艾府](https://leetcode.cn/problems/shortest-subarray-with-sum-at-least-k/solution/liang-zhang-tu-miao-dong-dan-diao-dui-li-9fvh/)

```java
class Solution {
    public int shortestSubarray(int[] nums, int k) {
        int n = nums.length;

        // 预处理前缀和数组
        long[] perSum = new long[n + 1];
        for (int i = 0; i < n; i ++) {
            perSum[i + 1] = perSum[i] + nums[i];
        }

        /**
         * 双端队列里存储的是已经遍历到的前缀和，这些元素在求子数组的和时作为左端点
         *
         *当遍历到 cur 时，对比队头的元素如果符合条件就将长度加入比较，
         * 此长度对于以队头元素能为左端点的子数组中是最短的（后续没有更短的符合条件的子数组了），因此弹出队头
         *
         * 在将 cur 加入队尾时，队尾元素可能是大于 cur 的，那么对于 q 中更前更大的元素是没有意义的，
         * 因为作为左端点越大越不容易符合条件，且即使符合条件也一定不如以当前元素为左端点的子数组的长度短（如果更大的元素符合条件，那么 cur 也一定符合条件）
         *
         * 保证了队列里是单调递增的
          */
        Deque<Integer> deque = new ArrayDeque<>();
        int res = n + 1;
        for (int i = 0; i <= n; i ++) {
            long cur = perSum[i];
            while (!deque.isEmpty() && cur - perSum[deque.peekFirst()] >= k) {
                res = Math.min(res, i - deque.pollFirst());
            }
            while (!deque.isEmpty() && perSum[deque.peekLast()] >= cur) {
                deque.pollLast();
            }
            deque.addLast(i);
        }
        return res < n + 1 ? res : -1;
    }
}
```

