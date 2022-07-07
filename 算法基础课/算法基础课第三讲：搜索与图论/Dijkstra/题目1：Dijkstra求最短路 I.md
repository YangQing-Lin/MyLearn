#### 题目1：<a href="https://www.acwing.com/problem/content/851/">Dijkstra求最短路 I</a>

------------------------

##### 题解：

###### c++

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 510;

int n, m;
int g[N][N];
int dist[N];
bool st[N];

int dijkstra() {
    memset(dist, 0x3f, sizeof(dist)); // 初始化每个点到起点的距离
    dist[1] = 0; // 起点到起点的距离为 0
    
    for (int i = 0; i < n; i ++) { // 遍历 n 次
        int t = -1; // 表示 t 还没有被赋值
        for (int j = 1; j <= n; j ++) { // 在所有点中，找到没有确定最短距离且距离最短的点
            if (!st[j] && (t == -1 || dist[t] > dist[j])) { 
                t = j;
            }
        }
        
        st[t] = true; // 表示上面循环中找到的点已经确定最短距离
        
        for (int j = 1; j <= n; j ++) { // 用这个已经确定最短距离的点更新其他的点的距离
            dist[j] = min(dist[j], dist[t] + g[t][j]);
        }
    }
    
    if (dist[n] == 0x3f3f3f3f) { // 如果 n 到起点的距离为无穷大，表示没有找到最短距离
        return -1;
    }
    else {
        return dist[n];
    }
}

int main() {
    cin >> n >> m;
    
    memset(g, 0x3f, sizeof(g)); // 初始化邻接矩阵
    
    while (m --) { // 读入 m 条边
        int a, b, c;
        cin >> a >> b >> c;
        
        g[a][b] = min(g[a][b], c); // 如果有重边，取其中权值最小的
    }
    
    int res = dijkstra();
    
    cout << res << endl;
    
    return 0;
}
```

###### java

```java
import java.util.Arrays;
import java.util.Scanner;

class Main {
    static int n, m;
    static int N = 510;
    static int[][] g = new int[N][N];
    static int[] dist = new int[N];
    static boolean[] st = new boolean[N];
    
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();
        m = scan.nextInt();
        
        for (int i = 1; i <= n; i++) Arrays.fill(g[i], 0x3f3f);
        
        while (m -- > 0) {
            int a, b, c;
            a = scan.nextInt();
            b = scan.nextInt();
            c = scan.nextInt();
            
            g[a][b] = Math.min(g[a][b], c);
        }
        
        int res = dijkstra();
        
        System.out.print(res);
    }
    
    public static int dijkstra() {
        Arrays.fill(dist, 0x3f3f);
        dist[1] = 0;
        
        for (int i = 0; i < n; i ++) {
            int t = -1;
            for (int j = 1; j <= n; j ++) {
                if (!st[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            
            st[t] = true;
            
            for (int j = 1; j <= n; j ++) {
                dist[j] = Math.min(dist[j], dist[t] + g[t][j]);
            }
        }
        
        if (dist[n] == 0x3f3f) {
            return -1;
        }
        else {
            return dist[n];
        }
    }
}
```

