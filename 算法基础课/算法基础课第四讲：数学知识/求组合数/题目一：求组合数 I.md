#### 题目一：<a href="https://www.acwing.com/problem/content/description/887/">求组合数 I</a>

--------------------

```java
import java.lang.reflect.Array;
import java.util.*;

public class Main {

    static int N = 2010;
    static int[][] c = new int[N][N];
    static final int MOD = (int) 1e9 + 7;


    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        
        init();
        
        while (n -- > 0) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            System.out.println(c[a][b]);
        }
    }
    
    // 预处理
    public static void init() {
        for (int i = 0; i < N; i ++) {
            for (int j = 0; j <= i; j ++) {
                if (j == 0) {
                    c[i][j] = 1;
                }
                else {
                    c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % MOD;
                }
            }
        }
    }
}
```

