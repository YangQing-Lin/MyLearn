#### <a href="https://www.acwing.com/problem/content/755/">平方矩阵 I</a>

------------

###### c++

```c++
#include <cstdio>
#include <iostream>

using namespace std;

int main() {
    int n;
    while (cin >> n, n) {
        for (int i = 1; i <= n; i ++) {
            for (int j = 1; j <= n; j ++) {
                int up = i, down = n - j + 1, left = j, right = n - i + 1;
                printf("%d ", min(min(up, down), min(left, right)));
            }
            puts("");
        }
        puts("");
    }
    
    return 0;
}
```

###### java

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (true) {
            int n = sc.nextInt();
            if (n == 0) {
                break;
            }

            for (int i = 1; i <= n; i ++) {
                for (int j = 1; j <= n; j ++) {
                    int up = i, down = n - j + 1, left = j, right = n - i + 1;
                    System.out.printf("%d ", Math.min(Math.min(up, down), Math.min(left, right)));
                }
                System.out.println();
            }
            System.out.println();
        }
    }
}
```

