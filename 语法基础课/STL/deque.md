#### deque

----------

双端队列

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <deque> // deque 在此头文件里

using namespace std;

int main() {
    // 常用函数
    clear(); // 清空
    front(); // 返回第一个元素
    back(); // 返回最后一个元素
    push_back(); // 向最后插入一个元素
    pop_back(); // 弹出最后一个元素
    push_front(); // 向队首插入一个元素
    pop_front(); // 弹出队首元素
    
    // 支持随机寻址
    
    // 迭代器
    begin();
    end();
    
    return 0;
}
```

