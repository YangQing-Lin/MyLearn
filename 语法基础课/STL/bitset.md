#### bitset

-----------

压位![压位](C:\Users\冬黎\OneDrive\图片\语法基础课\压位.png)

即每个字节存八位，则只需要开 128 个字节，bitset 就是让每个字节上存八位，比正常的 bool 数组要省八倍内存

--------------

##### 应用场景：

当我们存 10000 * 10000 的 bool 矩阵，则需要 10<sup>8</sup> 个 bool，需要大概 100 MB 的空间，但是限制空间是64MB，这时候就可以用 bitset 来存，bitset 可以省八倍空间，即只需要 12 MB 的空间就可以了![压位应用场景](C:\Users\冬黎\OneDrive\图片\语法基础课\压位应用场景.png)

-----------

```c++
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>

using namespace std;

int main() {
    // 定义
    bitset<10000> s; // 箭括号里写的是个数，即定义长度为10000的 bitset
    
    // 支持各种位运算操作
    ~s; // 取反
    &, |, ^; // 与，或，异或
    >>, <<; // 支持移位操作
    ==, !=; // 等号，不等号
    []; // 支持中括号操作，取出每一位是 0 还是 1
    
    // 常用函数
    count(); // 返回有多少个 1
    any(); // 判断是否至少有一个 1
    none(); // 判断全为 0
    set(); // 把所有位变成 1
    set(k, v); // 将第 k 位变成 v
    reset(); // 把所有位变成 0
    flip(); // 把所有位取反
    flip(k); // 把第 k 位取反
}
```

