#### 题目1：<a href="https://www.acwing.com/problem/content/837/">Trie 字符串统计</a>

------------------

##### 题解：

c++

```c++
#include <iostream> 

using namespace std;

const int N = 100010;

int son[N][26], res[N], idx; // son[N][26] 每个结点的孩子，res[N] 记录每个单词出现的次数，下标为 0 的点，既是根节点，也是空结点
char str[N];

void insert(char str[]) {
    int p = 0;
    for (int i = 0; str[i]; i ++) {
        int u = str[i] - 'a'; // 把 26 个字母映射到 0~25 的数字上
        if (!son[p][u]) {
            son[p][u] = ++ idx;
        }
        p = son[p][u];
    }
    
    res[p] ++;
}

int query(char str[]) {
    int p = 0;
    for (int i = 0; str[i]; i ++) {
        int u = str[i] - 'a';
        if (!son[p][u]) {
            return 0;
        }
        p = son[p][u];
    }
    
    return res[p];
}

int main() {
    int n;
    cin >> n;
    
    while (n --) {
        char op[2];
        cin >> op >> str;
        
        if (op[0] == 'I') {
            insert(str);
        }
        else {
            printf("%d\n", query(str));
        }
    }
    
    return 0;
}
```

java

```java
import java.util.*;

public class Main {

    private static int N = 100010, idx = 0;
    private static int[][] son = new int[N][26];
    private static int[] cnt = new int[N];

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int q = sc.nextInt();
        while (q -- > 0) {
            String ss = sc.next();
            String str = sc.next();
            if (ss.equals("I")) {
                insert(str);
            } else {
                System.out.println(query(str));
            }
        }
    }

    public static void insert(String x) {
        int p = 0;
        for (int i = 0; i < x.length(); i ++) {
            int u = x.charAt(i) - 'a';
            if (son[p][u] == 0) {
                son[p][u] = ++ idx;
            }
            p = son[p][u];
        }
        cnt[p] ++;
    }

    public static int query(String x) {
        int p = 0;
        for (int i = 0; i < x.length(); i ++) {
            int u = x.charAt(i) - 'a';
            if (son[p][u] == 0) {
                return 0;
            }
            p = son[p][u];
        }
        return cnt[p];
    }
}
```

