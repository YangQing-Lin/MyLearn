#### Floyd

------------

求多源汇最短路，不能处理含负权回路的图

基于动态规划

##### 算法思路：

1. 用邻接矩阵存储图   ```d[i][j]```
2. 三重循环
3. 循环之后，```d[i][j]``` 存的是 i 到 j 的最短距离 

<img src="https://raw.githubusercontent.com/DaoZuQieXing/Learn/main/img/算法基础课/算法基础课第三讲：搜索与图论/floyd算法.png" alt="system call" style="max-width: 70%">

