#### bellman-ford

-------------

处理图中可能存在负权回路的问题

当最短路问题中，存在负权边，最短路不一定存在

##### 思路：

1. for 循环 n 次
2. 每次遍历每条边，边用任意一个结构体来存
3. 更新最短距离![bellman-ford算法思路](C:\Users\冬黎\OneDrive\图片\算法基础课\算法基础课第三讲\bellman-ford算法思路.png)

循环完之后，可以证明所有的边都满足三角不等式![bellman-ford三角不等式](C:\Users\冬黎\OneDrive\图片\算法基础课\算法基础课第三讲\bellman-ford三角不等式.png)

