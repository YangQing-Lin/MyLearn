#### 题目1：<a href="https://www.acwing.com/problem/content/856/">Floyd求最短路</a>

------------

##### 题解：

###### c++

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 210, INF = 0x3f3f3f3f;

int n, m, Q;
int d[N][N];

void floyd() {
    for (int k = 1; k <= n; k ++) {
        for (int i = 1; i <= n; i ++) {
            for (int j = 1; j <= n; j ++) {
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
            }
        }
    }
}

int main() {
    cin >> n >> m >> Q;
    
    for (int i = 1; i <= n; i ++) {
        for (int j = 1; j <= n; j ++) {
            if (i == j) {
                d[i][j] = 0;
            }
            else {
                d[i][j] = INF;
            }
        }
    }
    
    while (m --) {
        int x, y, z;
        cin >> x >> y >> z;
        
        d[x][y] = min(d[x][y], z);
    }
    
    floyd();
    
    while (Q --) {
        int a, b;
        cin >> a >> b;
        
        if (d[a][b] > INF / 2) {
            puts("impossible");
        } 
        else {
            printf("%d\n", d[a][b]);
        }
    }
    
    return 0;
}
```

###### java

```java
import java.util.*;

class Main {
    static int n, m, Q;
    static int N = 210;
    static int max = 0x3f3f3f3f;
    static int[][] d = new int[N][N];
    
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();
        m = scan.nextInt();
        Q = scan.nextInt();
        
        for (int i = 1; i <= n; i ++) {
            for (int j = 1; j <= n; j ++) {
                if (i == j) {
                    d[i][j] = 0;
                }
                else {
                    d[i][j] = max;
                }
            }
        }
        
        while (m -- > 0) {
            int x, y, z;
            x = scan.nextInt();
            y = scan.nextInt();
            z = scan.nextInt();
            
            d[x][y] = Math.min(d[x][y], z);
        }
        
        floyd();
        
        while (Q -- > 0) {
            int a, b;
            a = scan.nextInt();
            b = scan.nextInt();
            
            if (d[a][b] > max / 2) {
                System.out.println("impossible");
            }
            else {
                System.out.println(d[a][b]);
            }
        }
    }
    
    public static void floyd() {
        for (int k = 1; k <= n; k ++) {
            for (int i = 1; i <= n; i ++) {
                for (int j = 1; j <= n; j ++) {
                    d[i][j] = Math.min(d[i][j], d[i][k] + d[k][j]);
                }
            }
        }
    }
}
```

