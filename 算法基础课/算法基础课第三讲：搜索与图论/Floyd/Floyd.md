#### Floyd

------------

求多源汇最短路，不能处理含负权回路的图

基于动态规划

##### 算法思路：

1. 用邻接矩阵存储图   ```d[i][j]```
2. 三重循环
3. 循环之后，```d[i][j]``` 存的是 i 到 j 的最短距离 

<img src="https://raw.githubusercontent.com/DaoZuQieXing/Learn/main/img/算法基础课/算法基础课第三讲：搜索与图论/floyd算法.png" alt="system call" style="max-width: 70%">

##### 算法原理：

基于动态规划

- 状态表示是三维：d[k, i, j] 表示从 i 点只经过 1~ k 这些中间点到达 j 的最短距离
- 状态转移：d[k, i, j] = d[k - 1, i, k] + d[k - 1, k, j]