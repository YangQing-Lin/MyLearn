#### 题目1：<a href="https://www.acwing.com/problem/content/861/">Kruskal算法求最小生成树</a>

-----------

##### 题解：

###### c++

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 200010;

int n, m;
int p[N];

struct items {
    int a, b, w;
    
    bool operator< (const items &W)const {
        return w < W.w;
    }
}item[N];

int find(int x) {
    if (p[x] != x) {
        p[x] = find(p[x]);
    }
    
    return p[x];
}

int main() {
    cin >> n >> m;
    
    for (int i = 0; i < m; i ++) {
        int a, b, w;
        cin >> a >> b >> w;
        
        item[i] = {a, b, w};
    }
    
    for (int i = 1; i <= n; i ++) {
        p[i] = i;
    }
    
    sort(item, item + m);
    
    int res = 0, cnt = 0;
    for (int i = 0; i < m; i ++) {
        int a = item[i].a, b = item[i].b, w = item[i].w;
        
        a = find(a), b = find(b);
        if (a != b) {
            p[a] = b;
            res += w;
            cnt ++;
        }
    }
    
    if (cnt < n - 1) {
        puts("impossible");
    }
    else {
        printf("%d\n", res);
    }
}
```

###### java

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.Scanner;

class Main {
    static int N = 200010;
    static int[] p = new int[N];
    static int n, m;
    
    public static class node {
        int a;
        int b;
        int c;
        public node(int a, int b, int c) {
            super();
            this.a = a;
            this.b = b;
            this.c = c;
        }
    }
    
    static Comparator<node> cmp = new Comparator<node>() {
        public int compare(node o1, node o2) { //将两边的权值按照从小到大排序
            return o1.c - o2.c;
        }
    };
    
    public static int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        
        return p[x];
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();
        m = scan.nextInt();
        
        node[] edges = new node[m];
        for(int i = 0; i < m; i ++) {
            int a = scan.nextInt();
            int b = scan.nextInt();
            int c = scan.nextInt();
            
            edges[i]=new node(a,b,c);
        }

        Arrays.sort(edges, cmp);

        for(int i = 1;i <= n;i ++) {
            p[i] = i;
        }

        int res = 0, cnt = 0;
        for(int i = 0; i < m; i ++) {
            int a = edges[i].a, b = edges[i].b, c = edges[i].c;
            
            a = find(a);
            b = find(b);
            if(a != b) {
                p[a] = b;
                res += c;
                cnt ++;
            }
        }

        if(cnt < n - 1) {
            System.out.println("impossible");
        }
        else {
            System.out.println(res);
        }
    }
}
```

