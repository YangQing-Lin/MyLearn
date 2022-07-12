#### 题目二：<a href="https://www.acwing.com/problem/content/888/">求组合数 II</a>

------------------

```java
import java.util.*;

public class Main{
    static int N = 100010;
    static int MOD = (int) 1e9 + 7;
    static long[] fact = new long[N]; // i的阶层
    static long[] infact = new long[N]; // i的阶层的逆元

    public static void main(String[] args){
        Scanner scan = new Scanner(System.in);

        init();

        int n = scan.nextInt();
        while(n -- > 0) {
            int a = scan.nextInt();
            int b = scan.nextInt();
            System.out.println(fact[a] * infact[b] % MOD * infact[a - b] % MOD);
        }
    }

    // 预处理阶乘和阶乘的逆元
    public static void init() {
        fact[0] = infact[0] = 1;
        for(int i = 1 ; i < N; i ++ ) {
            fact[i] = fact[i - 1] * i % MOD;
            infact[i] = infact[i - 1] * qmi(i,MOD - 2, MOD) % MOD;
        }
    }

    // 快速幂
    public static long qmi(long a,int k,int p){
        long res = 1;
        while(k != 0) {
            if((k & 1) == 1) {
                res = res * a % MOD;
            }
            k >>= 1;
            a = a * a % MOD;
        }
        return res;
    }
}
```

