#### 题目1：<a href="https://www.acwing.com/problem/content/860/">Prim算法求最小生成树</a>

------------

##### 题解：

###### c++

```c++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510, INF = 0x3f3f3f3f;

int n, m;
int g[N][N];
int dist[N];
bool st[N];

int prim()
{
    memset(dist, 0x3f, sizeof(dist));

    int res = 0;
    for (int i = 0; i < n; i ++ ) {
        int t = -1;
        for (int j = 1; j <= n; j ++ ) {
            if (!st[j] && (t == -1 || dist[t] > dist[j])) {
                t = j;
            }
        }

        if (i && dist[t] == INF) {
            return INF;
        }

        if (i) {
            res += dist[t];
        }
        
        st[t] = true;
        
        for (int j = 1; j <= n; j ++ ) {
            dist[j] = min(dist[j], g[t][j]);
        }
    }

    return res;
}


int main()
{
    cin >> n >> m;

    memset(g, 0x3f, sizeof(g));

    while (m -- ) {
        int a, b, c;
        cin >> a >> b >> c;
        
        g[a][b] = g[b][a] = min(g[a][b], c);
    }

    int t = prim();

    if (t == INF) {
        puts("impossible");
    }
    else {
        printf("%d\n", t);
    }

    return 0;
}
```

###### java

```java
import java.util.Arrays;
import java.util.Scanner;

public class Main {
    static int N = 510, INF = 0x3f3f3f3f;
    static int n, m;
    static int [][] g = new int[N][N];
    static int [] dist = new int[N];
    static boolean[] st = new boolean[N];

    private static int prim() {
        Arrays.fill(dist, INF);

        int res = 0;
        for (int i = 0; i < n; i ++) {
            int t = -1;
            for (int j = 1; j <= n; j ++) {
                if (!st[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            
            if (i != 0 && dist[t] == INF) {
                return INF;
            }
            
            if (i != 0) {
                res += dist[t];
            }
            st[t] = true;
                        
            for (int j = 1; j <= n; j ++) {
                dist[j] = Math.min(dist[j], g[t][j]);
            }
        }
        
        return res;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        n = sc.nextInt();
        m = sc.nextInt();
        for (int i = 0; i < N; i++) {
            Arrays.fill(g[i], INF);
        }
        while (m -- > 0) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            int c = sc.nextInt();
            g[a][b] = g[b][a] = Math.min(g[a][b], c);
        }

        int t = prim();
        if (t == INF) {
            System.out.println("impossible");
        }
        else {
            System.out.println(t);
        }
    }
}
```

