#### 题目一：<a href="https://www.acwing.com/problem/content/893/">Nim游戏</a>

-----------------

###### 直接看初态异或值即可判断先手是否必胜

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int res = 0;
        while (n -- > 0) {
            int a = sc.nextInt();
            res ^= a;
        }

        if (res != 0) {
            System.out.println("Yes");
        }
        else {
            System.out.println("No");
        }
    }
}
```

