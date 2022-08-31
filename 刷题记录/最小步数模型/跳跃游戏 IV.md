#### <a href="https://leetcode.cn/problems/jump-game-iv/">跳跃游戏 IV</a>

---------

##### 最小步数模型

一般抽象成图论问题

直接爆搜会超时，因为有相同的点，这些点之间都有一条边，因此**需要优化相同值的点之间的扩展**

对于这些值相同的点，如果扩展其中任何一个点，其他所有的点都会更新距离

##### bfs求最短路径问题的一个重要性质：**每个点第一次更新的时候才是最短距离**

因此，开一个哈希表记录（key：相同的值，value：相同值的下标），每次扩展完相同值，就将它从哈希表里删除，**每个相同的值的点只更新第一次**

```java
class Solution {
    public int minJumps(int[] arr) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        int n = arr.length;
        for (int i = 0; i < n; i ++) {
            int val = arr[i];
            if (map.get(val) == null) {
                map.put(val, new ArrayList<>());
            }
            map.get(val).add(i);
        }

        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        Queue<Integer> queue = new LinkedList<>();
        queue.add(0);
        dist[0] = 0;
        while (!queue.isEmpty()) {
            int t = queue.poll();

            for (int i = t - 1; i <= t + 1; i += 2) { // 扩展相邻的点
                if (i >= 0 && i < n && dist[i] > dist[t] + 1) {
                    dist[i] = dist[t] + 1;
                    queue.add(i);
                }
            }

            int val = arr[t];
            if (map.containsKey(val)) { // 如果哈希表里存在，说明没有扩展过，相同的值只扩展一次
                for (int i : map.get(val)) {
                    if (dist[i] > dist[t] + 1) {
                        dist[i] = dist[t] + 1;
                        queue.add(i);
                    }
                }
                map.remove(val);
            }
        }
        return dist[n - 1];
    }
}
```

