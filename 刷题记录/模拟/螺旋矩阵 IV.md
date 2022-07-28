#### <a href="https://leetcode.cn/problems/spiral-matrix-iv/">螺旋矩阵 IV</a>

----------------

蛇形矩阵扩展版

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public int[][] spiralMatrix(int m, int n, ListNode head) {
        int[][] res = new int[m][n];
        for (int i = 0; i < m; i ++) {
            for (int j = 0; j < n; j ++) {
                res[i][j] = -1;
            }
        }

        int[] dx = {-1, 0, 1, 0}, dy = {0, 1, 0, -1};

        int x = 0, y = 0, d = 1;
        for (int i = 0; i < n * m && head != null; i ++) {
            res[x][y] = head.val;

            int a = x + dx[d], b = y + dy[d];
            if (a < 0 || a >= m || b < 0 || b >= n || res[a][b] != -1) {
                d = (d + 1) % 4;
                a = x + dx[d];
                b = y + dy[d];
            }
            x = a;
            y = b;
            head = head.next;
        }

        return res;
    }
}
```

