#### <a href="https://leetcode.cn/problems/total-cost-to-hire-k-workers/">雇佣 K 位工人的总代价</a>

---------------

使用两个小根堆维护前部和后部的集合最小值即可

```java
class Solution {
    public long totalCost(int[] arr, int k, int cd) {
        int n = arr.length;
        int i = 0, j = n - 1;
        long res = 0;
        PriorityQueue<Integer> heap1 = new PriorityQueue<>(); // 前部
        PriorityQueue<Integer> heap2 = new PriorityQueue<>(); // 后部
        while (k != 0) {
            while (heap1.size() < cd && i <= j) heap1.offer(arr[i ++]);
            while (heap2.size() < cd && i <= j) heap2.offer(arr[j --]);

            int a = heap1.isEmpty() ? Integer.MAX_VALUE : heap1.peek();
            int b = heap2.isEmpty() ? Integer.MAX_VALUE : heap2.peek();
            if (a <= b) {
                res += a;
                heap1.poll();
            } else {
                res += b;
                heap2.poll();
            }
            k --;
        }
        return res;
    }
}
```

