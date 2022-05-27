#### set

------------------

包括 set 和 multiset

set 和 multiset 的区别：

1. set 里面不能有重复元素，如果插入了重复元素，那么这个操作就会被忽略掉
2. multiset 里面是可以有重复元素的    

```c++
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>
#include <set> // set 和 multiset 在此头文件里

using namespace std;

int main() {
    // 定义
    set<int> S;
    multiset<int> MS;
    
    // 常用函数
    clear(); // 清空
    insert(); // 插入一个数
    find(); // 查找一个数，如果不存在，返回 end() 这个迭代器
    count(); // 返回某一个数的个数
    erase(x); // 输入是一个数 x，删除所有 x  时间复杂度是 O(k + logn)  k 是 x 的个数
    erase(); // 输入是迭代器，删除这个迭代器
    
    // 核心操作
    lower_bound(); // 返回大于等于 x 的最小的数的迭代器   如果不存在返回 end()
    upper_bound(); // 返回大于 x 的最小的数的迭代器      如果不存在返回 end()
    
    // 迭代器，支持 ++, -- 操作
    begin();
    end();
    begin()/end() ++, -- // 返回前驱和后继
        
    return 0;
}
```

