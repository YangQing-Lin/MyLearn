#### 题目2：<a href="https://www.acwing.com/problem/content/854/">spfa判断负环</a>

------------------

开一个数组表示最短路上的边数，在更新距离的时候，同时更新这个边数

当这个边数大于或等于 n，由抽屉原理可知，图中含有环，又因为是最短路上的环，所以这个环的权值一定是负的，即负环

<img src="https://raw.githubusercontent.com/DaoZuQieXing/Learn/main/img/算法基础课/算法基础课第三讲：搜索与图论/spfa判断负环.png" alt="system call" style="max-width: 70%">

------------------

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
int dist[N], cnt[N];
bool st[N];

bool spfa() {
    queue<int> q;
    for (int i = 1; i <= n; i ++) {
        q.push(i);
        st[i] = true;
    }
    
    while (q.size()) {
        int t = q.front();
        q.pop();
        
        st[t] = false;
        
        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                cnt[j] = cnt[t] + 1;
                
                if (!st[j]) {
                    q.push(j);
                    st[j] = true;
                }
                
                if (cnt[j] >= n) {
                    return true;
                } 
            }
        }
    }
    
    return false;
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
    
    if (spfa()) {
        puts("Yes");
    }
    else {
        puts("No");
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
    static int[] cnt = new int[N];
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
        
        if (spfa()) {
            System.out.println("Yes");
        }
        else {
            System.out.println("No");
        }
    }
    
    public static void add(int a, int b, int c) {
        e[idx] = b;
        w[idx] = c;
        ne[idx] = h[a];
        h[a] = idx ++;
    }
    
    public static boolean spfa() {
        Queue<Integer> q = new LinkedList<>();
        for (int i = 1; i <= n; i ++) {
            q.offer(i);
            st[i] = true;
        }
        
        while (q.size() > 0) {
            int t = q.poll();
            
            st[t] = false;
            
            for (int i = h[t]; i != -1; i = ne[i]) {
                int j = e[i];
                if (dist[j] > dist[t] + w[i]) {
                    dist[j] = dist[t] + w[i];
                    cnt[j] = cnt[t] + 1;
                    
                    if (!st[j]) {
                        q.offer(j);
                        st[j] = true;
                    }
                    
                    if (cnt[j] >= n) {
                        return true;
                    }
                }
            }
        }
        
        return false;
    }
}
```

