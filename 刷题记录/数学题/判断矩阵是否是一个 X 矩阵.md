#### <a href="https://leetcode.cn/problems/check-if-matrix-is-x-matrix/">判断矩阵是否是一个 X 矩阵</a>

-------------

如果满足 $i = j$ 或者 $i + j + 1 = n$，则说明该点在矩阵 $grid$ 的对角线上，否则不在矩阵对角线上。

```java
class Solution {
    public boolean checkXMatrix(int[][] grid) {
        int n = grid.length;
        for (int i = 0; i < n; i ++) {
            for (int j = 0; j < n; j ++) {
                if ((i == j) || (i + j + 1 == n)) {
                    if (grid[i][j] == 0) return false;
                } else {
                    if (grid[i][j] != 0) return false;
                }
            }
        }
        return true;
    }
}
```

##### 简化代码

像这种需要判断「两个条件都为真」「两个条件都为假」的逻辑，都可以直接用这两个条件的 bool 值作比较，从而简化代码逻辑：

```java
class Solution {
    public boolean checkXMatrix(int[][] grid) {
        int n = grid.length;
        for (int i = 0; i < n; i ++) {
            for (int j = 0; j < n; j ++) {
                if ((i == j || i + j + 1 == n) == (grid[i][j] == 0)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

