#### <a href="https://www.acwing.com/problem/content/description/4279/)">擅长C</a>

---------------

###### c++

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

char g[26][7][6];
bool isFirst = true;

void output(string word) {
    if (word.size() == 0) { // 如果单词为空，则停止
        return;
    }
    
    if (isFirst) { // 除了第一个单词，每个单词前都有一个回车
        isFirst = false;
    }
    else {
        cout << endl;
    }
    
    // 读入单词需要的字母模板
    char str[7][60] = {0};
    for (int i = 0; i < word.size(); i ++) {
        for (int j = 0; j < 7; j ++) {
            for (int k = 0; k < 5; k ++) {
                str[j][i * 6 + k] = g[word[i] - 'A'][j][k];
            }
        }
    }
    
    // 每个字母间有一列空格
    for (int i = 1; i < word.size(); i ++) {
        for (int j = 0; j < 7; j ++) {
            str[j][i * 6 - 1] = ' ';
        }
    }
    
    // 输出每个单词的模板
    for (int i = 0; i < 7; i ++) {
        cout << str[i] << endl;
    }
}

int main() {
    // 把每个字母的模板记下来
    for (int i = 0; i < 26; i ++) {
        for (int j = 0; j < 7; j ++) {
            cin >> g[i][j];
        }
    }
    
    // 读入要用模板输出的单词
    string word;
    char c;
    while ((c = getchar()) != -1) { 
        if (c >= 'A' && c <= 'Z') {
            word += c;
        }
        else {
            output(word);
            word = "";
        }
    }
    
    // 输出最后一个单词，因为最后一个单词后有文件结束符 -1，因此 word 读取完最后一个单词后就跳出循环了
    output(word); 
    
    return 0;
}
```

###### java

```java

```

