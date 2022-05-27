#### map

---------

包括 map 和 multimap

```c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <map> // map 和 multimap 在此头文件里

using namespace std;

int main() {
    // 常用函数
    clear(); // 清空
    insert(); // 插入的数是一个 pair
    erase(); // 输入的参数是 pair 或者 迭代器
    find(); // 查找一个数，如果不存在，返回 end() 这个迭代器
    lower_bound(); // 返回大于等于 x 的最小的数的迭代器   如果不存在返回 end()
    upper_bound(); // 返回大于 x 的最小的数的迭代器      如果不存在返回 end()
    
    // 核心操作，像用数组一样，用 map，时间复杂度是 O(logn) 
    map<string, int> a; // 定义，把 string 映射成 int
    a["dl"] = 1; // 插入，dl 映射到了 1
    cout << a["yxc"] << endl; // 查找
    
    // 迭代器，支持 ++, -- 操作
    begin();
    end();
    begin()/end() ++, -- // 返回前驱和后继，时间复杂度是 O(logn)
        
    return 0;    
}
```

