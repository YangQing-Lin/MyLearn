#### unordered

--------

包括 unordered_set，unordered_map，unordered_multiset，unordered_multimap

和 set map 类似，增删改查的时间复杂度是 O(1)

但是不支持 ```lower_bound(x) / upper_bound(x)```，其内部是没有序的，且不支持迭代器的 ++ --

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <unordered_map> // unordered_map 在此头文件里

using namespace std;

int main() {
    unordered_multimap<string, int> a;
    
    return 0;
}
```

