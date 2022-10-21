#### <a href="https://leetcode.cn/problems/find-nearest-point-that-has-the-same-x-or-y-coordinate/">找到最近的有相同 X 或 Y 坐标的点</a>

-------------

```java
class Solution {
    public int nearestValidPoint(int x, int y, int[][] points) {
        int pos = -1, dist = Integer.MAX_VALUE;
        for (int i = 0; i < points.length; i ++) {
            int a = points[i][0], b = points[i][1];
            if (a == x || b == y) {
                int t = Math.abs(x - a) + Math.abs(y - b);
                if (t < dist) {
                    pos = i;
                    dist = t;
                }
            }
        }
        return pos;
    }
}
```

