#### <a href="https://leetcode.cn/problems/jump-game-iii/">跳跃游戏 III</a>

-------------

```java
class Solution {
    public boolean canReach(int[] arr, int start) {
        boolean[] st = new boolean[arr.length];
        Arrays.fill(st, false);
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(start);
        while (!queue.isEmpty()) {
            int t = queue.poll();
            if (arr[t] == 0) return true;
            int dist = arr[t];
            if (t + dist < arr.length && !st[t + dist]) {
                queue.offer(t + dist);
                st[t + dist] = true;
            }
            if (t - dist >= 0 && !st[t - dist]) {
                queue.offer(t - dist);
                st[t - dist] = true;
            }
        }
        return false;
    }
}
```

