#### 题目2：<a href="https://www.acwing.com/problem/content/852/">Dijkstra求最短路 II</a>

------------------

##### 堆优化版

在朴素版本中，最慢的一步是 for 循环中，找到不在 s 中且距离最近的点，这一步是 n<sup>2</sup> 的，则用堆来优化这一步

用堆优化之后，这一步的时间复杂度会变成 O(1) 的

![](C:\Users\冬黎\OneDrive\桌面\Learn\img\算法基础课\算法基础课第三讲：搜索与图论\堆优化Dijkstra算法.png)

-----------

##### 题解：

###### c++

```c++
#include <iostream>
#include <cstring>
#include <queue>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

const int N = 1e6 + 10;

int n, m;
int h[N], w[N], e[N], ne[N], idx;
int dist[N];
bool st[N];

int dijkstra() {
    memset(dist, 0x3f, sizeof(dist));
    dist[1] = 0;
    
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});
    
    while (heap.size()) {
        auto t = heap.top();
        heap.pop();
        
        int var = t.second, distance = t.first;
        
        if (st[var]) {  // 说明是堆中的冗余备份
            continue;
        }
        else {
            st[var] = true;
        }
        
        for (int i = h[var]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > distance + w[i]) {
                dist[j] = distance + w[i];
                heap.push({dist[j], j});
            }
        }
    }
    
    if (dist[n] == 0x3f3f3f3f) {
        return -1;
    }
    else {
        return dist[n];
    }
}

void add(int a, int b, int c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = h[a];
    h[a] = idx;
    idx ++;
}

int main() {
    cin >> n >> m;
    
    memset(h, -1, sizeof(h));
    
    while (m --) {
        int a, b, c;
        cin >> a >> b >> c;
        
        add(a, b, c);
    }
    
    int res = dijkstra();
    
    cout << res << endl;
    
    return 0;
}
```

###### java

```java
import java.io.*;
import java.lang.*;
import java.util.*;

class Main {
    static int n = 0, m = 0, N = 1000010;
    static PriorityQueue<int[]> q = new PriorityQueue<>((a,b)->{return a[1] - b[1];});//堆
    static int[] dist = new int[N];//距离数组
    static boolean[] st = new boolean[N];//标记数组
    static int[] h = new int[N], ne = new int[N], e = new int[N], w = new int[N];//邻接表
    static int idx = 0;
    
    public static int dijkstra() {
        Arrays.fill(dist, 0x3f3f3f3f);
        dist[1] = 0;//初始化第一个点到自身的距离
        
        q.offer(new int[]{1, 0});
        
        while(q.size() != 0) {
            int[] a = q.poll();
            
            int t = a[0], distance = a[1];
            
            if (st[t]) {
                continue;
            }
            else {
                st[t] = true;
            }
            
            for(int i = h[t]; i != -1; i = ne[i]) {
               int j = e[i];
               if(dist[j] > distance + w[i]) {
                   dist[j] = distance + w[i];
                   q.offer(new int[]{j, dist[j]});
               }
            }
        }
        
        if (dist[n] == 0x3f3f3f3f) {
            return -1;
        }
        else {
            return dist[n];
        }
    }
    
    public static void add(int a, int b, int c){
        e[idx] = b;
        w[idx] = c;
        ne[idx] = h[a];
        h[a] = idx;
        idx ++;
    }
    
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();
        m = scan.nextInt();
        
        Arrays.fill(h, -1);
        
        while (m -- > 0) {
            int a = scan.nextInt();
            int b = scan.nextInt();
            int c = scan.nextInt();
            add(a, b, c);
        }
        
        System.out.print(dijkstra());
    }
}
```

