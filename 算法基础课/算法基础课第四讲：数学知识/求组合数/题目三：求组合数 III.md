#### 题目三：<a href="https://www.acwing.com/problem/content/889/">求组合数 III</a>

-----------------

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        while(n -- > 0) {
            long a = scanner.nextLong();
            long b = scanner.nextLong();
            int p = scanner.nextInt();
            System.out.println(lucas(a, b, p));
        }
    }
    
    // 卢卡斯定理
    private static long lucas(long a, long b, int p) {
        if(a < p && b < p) {
            return C(a, b, p);
        }
        return C(a % p, b % p, p) * lucas(a / p, b / p, p) % p;
    }
    
    // 求阶乘和阶乘的逆元
    private static long C(long a, long b, int p) {
        if(b > a) {
            return 0;
        }
        long res = 1, i, j;
        for(i = 1, j = a; i <= b; i ++, j--) {
            res = res * j % p;
            res = res * qmi(i, p - 2, p) % p;
        }
        return res;
    }
    
    // 快速幂
    private static long qmi(long a, int k, int m) {
        long res = 1;
        while(k > 0) {
            if((k & 1) == 1)res = res * a % m;
            k >>= 1;
            a = a * a % m;
        }
        return res;
    }
}
```

