#### Dijkstra

---------------

##### 朴素版本：

1. 先初始化距离，即 dist[1] = 0，dist[i] = 正无穷，定义一个集合 s，把已经确定最短路的点放到 s 里
2. for 循环，先找到不在 s 中的距离最近的点 t，然后把 t 放到 s 中，再用 t 更新其他点的距离
3. 循环之后，可以得到每个点到起点的最短距离

<img src="https://raw.githubusercontent.com/DaoZuQieXing/Learn/main/img/算法基础课/算法基础课第三讲：搜索与图论/朴素Dijkstra.png" alt="system call" style="max-width: 70%">

