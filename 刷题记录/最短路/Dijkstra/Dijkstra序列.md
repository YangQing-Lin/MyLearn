#### <a href="https://www.acwing.com/problem/content/4278/">Dijkstra序列</a>

-------------

###### c++

```c++
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 1010;

int n, m;
int dist[N], g[N][N];
bool st[N];
int q[N];

bool dijkstra() {
    memset(dist, 0x3f3f, sizeof(dist));
    dist[q[0]] = 0; // 起点的距离为0
    memset(st, 0, sizeof(st));
    for (int i = 0; i < n; i ++) {
        int t = q[i];
        for (int j = 1; j <= n; j ++) {
            if (!st[j] && dist[j] < dist[t]) {
                return false;
            }
        }
        
        st[t] = true;
        
        for (int i = 1; i <= n; i ++) {
            dist[i] = min(dist[i], dist[t] + g[t][i]);
        }
    }
}

int main() {
    cin >> n >> m;
    
    memset(g, 0x3f3f, sizeof(g));
    while (m --) {
        int a, b, c;
        cin >> a >> b >> c;
        g[a][b] = g[b][a] =  c;
    }
    
    int k;
    cin >> k;
    while (k --) {
        for (int i = 0; i < n; i ++) {
            cin >> q[i];
        }
        if (dijkstra()) {
            puts("Yes");
        }
        else {
            puts("No");
        }
    }
    
    return 0;
}
```

###### java

```java
import java.util.*;

class Main {
    static int n, m, k;
    static int N = 1010;
    static int[] dist = new int[N];
    static int[] q = new int[N];
    static int[][] g = new int[N][N];
    static boolean[] st = new boolean[N];
    
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();
        m = scan.nextInt();
        
        for (int i = 0 ; i < N ; i ++ ) {
            Arrays.fill(g[i], 0x3f3f3f3f);
        }
            
        while (m -- > 0) {
            int a, b, c;
            a = scan.nextInt();
            b = scan.nextInt();
            c = scan.nextInt();
            
            g[a][b] = g[b][a] = c;
        }
        
        k = scan.nextInt();
        while (k -- > 0) {
            for (int i = 0; i < n; i ++) {
                q[i] = scan.nextInt();
            }
            if (dijkstra()) {
                System.out.println("Yes");
            }
            else {
                System.out.println("No");
            }
        }
    } 
    
    public static boolean dijkstra() {
        Arrays.fill(dist, 0x3f3f3f3f);
        dist[q[0]] = 1;
        Arrays.fill(st, false);
        for (int i = 0; i < n; i ++) {
            int t = q[i];
            for (int j = 1; j <= n; j ++) {
                if (!st[j] && dist[j] < dist[t]) {
                    return false;
                }
            }
            
            st[t] = true;
            
            for (int k = 1; k <= n; k ++) {
                dist[k] = Math.min(dist[k], dist[t] + g[t][k]);
            }
        }
        
        return true;
    }
}
```

