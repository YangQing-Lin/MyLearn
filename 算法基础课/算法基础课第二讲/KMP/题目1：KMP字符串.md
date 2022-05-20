#### 题目1：<a href="https://www.acwing.com/problem/content/833/">KMP字符串</a>

---------------

##### 思路：

1. 求出next数组，用 p 匹配 p，得到每个点前缀和后缀的最大值，即是每次往后退的最小值
2. KMP 匹配过程，枚举 s 每个数，如果匹配就 比较下一个，不匹配就推到 next [j]
3. 当 j 枚举完 p 之后，输出起点下标

---------------------------

##### 题解：

```c++
#include <iostream>

using namespace std;

const int N = 100010, M = 1000010;

int n, m;
char p[N], s[M];
int ne[N];

int main() {
    cin >> n >> p + 1 >> m >> s + 1;
    
    // 求 ne[j]
    for (int i = 2, j = 0; i <= n; i ++) {
        while (j && p[i] != p[j + 1]) {
            j = ne[j];
        }
        if (p[i] == p[j + 1]) {
            j ++;
        }
        ne[i] = j;
    }
    
    // KMP 匹配
    for (int i = 1, j = 0; i <= m; i ++) {
        while (j && s[i] != p[j + 1]) {
            j = ne[j];
        }
        if (s[i] == p[j + 1]) {
            j ++;
        }
        if (j == n) {
            printf("%d ", i - n);
            j = ne[j];
        }
    }
    
    return 0;
}
```

