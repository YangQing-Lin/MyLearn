#### string

-----------

字符串

```c++
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>

using namespace std;

int main() {
    string a = "dl";
    
    a += "def"; // 可以在后面加上一个字符串
    a += 'c'; // 也可以在后面加上一个字符
    
    // 常用的函数
    clear(); // 清空
    a.substr(); // 返回的是某一个子串，有两个参数，第一个参数是子串的起始位置，第二个参数是子串的长度
    a.substr(1, 10); // 当第二个参数超出字符串长度的时候，就会输出到最后一个字母为止，也可以把第二个参                      // 数省略，就会返回从第一个参数开始的整个子串
    a.length(); // 和 a.size() 一样，都是返回字符串长度
    
    // 当我们想用 printf 输出一个 string 的时候
    printf("%s\n", a.c_str()); // 要输出的其实是字符数组的起始地址，就可以用 c_str() 返回 a 存储字                                // 符串的字符数组的起始地址
	
    return 0;
}
```

