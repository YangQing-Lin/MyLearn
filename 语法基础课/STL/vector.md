#### vector

----------

##### 一些基本操作

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cstdio>
#include <vector> // vector 在此头文件里

using namespace std;

int main() {
    // vector 的几种初始化方式初始化
    vector<int> a; // 定义一个 vector
    vector<int> a(10); // 定义一个长度为 10 的 vector
    vector<int> a(10, 3); // 定义一个长度为 10 的 vector，里面的每个元素都是 3
    vector<int> a[10]; // 定义了 10 个 vector
    
    // vector 的常用函数
    clear(); // 清空
    front(); // 返回 vector 的第一个元素
    back(); // 返回 vector 的最后一个元素
    push_back(); // 向 vector 的最后插入一个元素
    pop_back(); // 删掉 vector 的最后一个元素
    
    // vector 的迭代器 迭代器可以看成指针
    begin(); // vector 的第 0 个数
    end(); // vector 的最后一个数的后面一个数
    a.begin(); // 即 a[0]
    a.end(); // 即 a[a.size()]
    
    // vector 的遍历方式 支持随机寻址
    for (int i = 0; i < a.size(); i ++) { // 用数组下标来遍历
        cout << a[i] << endl;
    }
    for (vector<int>::iterator i = a.begin(); i != a.end(); i ++) { // 用迭代器遍历
        cout << *i << endl; // 解引用，取 i 地址里的内容，vector<int>::iterator 可以写成 auto
    } 
    for (auto x : a) { // 范围遍历
        cout << x << ' ';
    }
    
    // vector 支持比较运算，按字典序
    vector<int> a(4, 3), b(3, 4);
    if (a < b) { // 可以判断两个 vector 的大小，按字典序来比较
        puts("a < b"); // 按照字典序，4 个 3 是小于 3 个 4 的
    }
}
```

-------

##### 倍增的思想

在 c++ 里，或者说在操作系统里，系统为某一个程序分配空间时，所需的时间基本上与空间大小无关，而是与申请次数有关

因为有以上这个特点，所以变长数组要尽量减少申请空间的次数

则在申请空间的时候，vector 的优化目标是减少申请的次数，但是可以浪费空间，所以运用了倍增的思想

###### 基本思路

一开始先分配一个长度为 32 的空间，当元素的个数大于 32 的时候，再申请一个长度为 64 的空间，然后把前面 32个元素 copy 到长度为 64 的空间里，即每次数组长度不够的时候，就把数组长度乘以 2，然后把之前的元素 copy 过来

###### 倍增的优点

申请的次数不多，额外的操作不多

---------------

##### pair

存储一个二元组

```c++
// 定义的方式，pair 的前后两个变量类型可以任意
pair<int, string> p;

p.first; // 取得 pair 的第一个元素
p.second; // 取得 pair 的第二个元素

// pair 支持比较运算，以 first 为第一关键字，second 为第二关键字（字典序）

// 构造一个 pair
p = make_pair(10, "dl");
p = {20, "yxc"};

// 用 pair 来存三个不同的属性
pair<int, pair<int, int>> p;
```

###### pair 的用途

假设某个东西有两种不同的属性，就可以用一个 pair 来存，需要按照某一种属性来排序，则把需要排序的关键字放到 first 里面，把不需要排序的关键字放到 second 里面，放在一起之后，对整个 pair 排序即可