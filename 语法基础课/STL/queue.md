#### queue

---------

队列

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <queue> // queue 在此头文件里

using namespace std;

int main() {
    queue<int> q;
    
    // 常用函数
    push(); // 向队尾插入一个元素
    front(); // 返回队头元素
    back(); // 返回队尾元素
    pop(); // 弹出队头元素
    q = queue<int>(); // 清空队列，即重新构造一个队列
    
    return 0;
}
```

 

