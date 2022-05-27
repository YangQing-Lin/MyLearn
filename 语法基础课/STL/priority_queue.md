#### priority_queue

-----------

优先队列，即堆，默认是大根堆

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <queue> // priority_queue 在此头文件里
#include <vector> // 直接定义小根堆时，需要加上此头文件

using namespace std;

int main() {
    // 定义
    priority_queue<int> heap;
    
    // 如何定义小根堆
    heap.push(-x); // 插入的时候把插入 -x 即可，因为把 -x 按照从大到小的顺序排序，就是把 x 按照从小                    // 到大的顺序排序
    priority_queue<int, vector<int>, greater<int>> heap; // 直接定义小根堆
    
    // 常用函数
    push(); // 插入一个元素
    top(); // 返回堆顶元素
    pop(); // 弹出堆顶元素
    
    return 0;
}
```

