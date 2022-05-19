#### scanf 和 cin 的区别

-------------

##### 原题：<a href="https://www.acwing.com/problem/content/720/">实验</a>

##### 题解：

```c++
#include <cstdio>
#include <iostream>

using namespace std;

int main()
{
    int n;
    cin >> n;

    int c = 0, r = 0, f = 0;
    for (int i = 0; i < n; i ++ )
    {
        int k;
        char t;
        scanf("%d %c", &k, &t);  // scanf在读入字符时，不会自动过滤空格、回车、tab
        if (t == 'C') c += k;
        else if (t == 'R') r += k;
        else f += k;
    }

    int s = c + r + f;
    // 输出百分号要用百分号转义，即两个百分号
    printf("Total: %d animals\n", s);
    printf("Total coneys: %d\n", c);
    printf("Total rats: %d\n", r);
    printf("Total frogs: %d\n", f);
    // 两个整数相除得到整数，想要得到浮点数，需要把其中一个转换为浮点数
    printf("Percentage of coneys: %.2lf %%\n", (double)c / s * 100);
    printf("Percentage of rats: %.2lf %%\n", (double)r / s * 100);
    printf("Percentage of frogs: %.2lf %%\n", (double)f / s * 100);

    return 0;
}
```

