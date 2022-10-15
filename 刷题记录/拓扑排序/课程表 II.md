#### <a href="https://leetcode.cn/problems/course-schedule-ii/">课程表 II</a>

-------------

```java
class Solution {

    int[] h;
    int[] e;
    int[] ne;
    int idx = 0;
    int[] dist;
    int[] q;
    int N = 100010;

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // 建图
        h = new int[N]; e = new int[N]; ne = new int[N]; dist = new int[N];
        Arrays.fill(h, -1);
        for (int i = 0; i < prerequisites.length; i ++) {
            int b = prerequisites[i][0], a = prerequisites[i][1];
            add(a, b);
            dist[b] ++;
        }

        if (topsort(numCourses)) {
            int[] res = new int[numCourses];
            for (int i = 0; i < numCourses; i ++) {
                res[i] = q[i];
            }
            return res;
        }
        else return new int[0];
    }

    public boolean topsort(int n) {
        q = new int[N];
        int hh = 0, tt = -1;
        for (int i = 0; i < n; i ++) {
            if (dist[i] == 0) {
                q[++ tt] = i;
            }
        }

        while (hh <= tt) {
            int t = q[hh ++];

            for (int i = h[t]; i != -1; i = ne[i]) {
                int j = e[i];
                dist[j] --;
                if (dist[j] == 0) q[++ tt] = j;
            }
        }
        return tt == n - 1;
    }

    public void add(int a, int b) {
        e[idx] = b;
        ne[idx] = h[a];
        h[a] = idx ++;
    }
}
```

