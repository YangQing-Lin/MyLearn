#### 题目1：<a href="https://www.acwing.com/problem/content/853/">spfa求最短路</a>

--------

##### 题解：

###### c++

```c++
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 100010;

int n, m;
int h[N], e[N], ne[N], w[N], idx;
int dist[N];
bool st[N];

int spfa() {
    memset(dist, 0x3f, sizeof(dist));
    dist[1] = 0;
    
    queue<int> q;
    q.push(1);
    
    st[1] = true;
    
    while (q.size()) {
        int t = q.front();
        q.pop();
        
        st[t] = false;
        
        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    
    return dist[n];
}

int add(int a, int b, int c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = h[a];
    h[a] = idx;
    idx ++;
}

int main() {
    cin >> n >> m;
    
    memset(h, -1, sizeof(h));
    
    for (int i = 0; i < m; i ++) {
        int a, b, c;
        cin >> a >> b >> c;
        
        add(a, b, c);
    }
    
    int t = spfa();
    
    if (t == 0x3f3f3f3f) {
        puts("impossible");
    }
    else {
        printf("%d", t);
    }
    
    return 0;
}
```

###### java

```java
import java.util.Arrays;
import java.util.Scanner;
import java.util.Queue;
import java.util.LinkedList;

class Main {
    static int n, m, idx;
    static int N = 100010, max = 0x3f3f3f3f;
    static int[] h = new int[N];
    static int[] e = new int[N];
    static int[] ne = new int[N];
    static int[] w = new int[N];
    static int[] dist = new int[N];
    static boolean[] st = new boolean[N];
    
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();
        m = scan.nextInt();
        
        Arrays.fill(h, -1);
        
        while (m -- > 0) {
            int a, b, c;
            a = scan.nextInt();
            b = scan.nextInt();
            c = scan.nextInt();
            
            add(a, b, c);
        }
        
        int t = spfa();
        
        if (t == max) {
            System.out.println("impossible");
        }
        else {
            System.out.println(t);
        }
    }
    
    public static void add(int a, int b, int c) {
        e[idx] = b;
        w[idx] = c;
        ne[idx] = h[a];
        h[a] = idx ++;
    }
    
    public static int spfa() {
        Arrays.fill(dist, max);
        dist[1] = 0;
        
        Queue<Integer> q = new LinkedList<>();
        q.offer(1);
        
        st[1] = true;
        
        while (q.size() > 0) {
            int t = q.poll();
            
            st[t] = false;
            
            for (int i = h[t]; i != -1; i = ne[i]) {
                int j = e[i];
                if (dist[j] > dist[t] + w[i]) {
                    dist[j] = dist[t] + w[i];
                    if (!st[j]) {
                        q.offer(j);
                        st[j] = true;
                    }
                }
            }
        }
        
        if (dist[n] == max) {
            return max;
        }
        else {
            return dist[n];
        }
    }
}
```

