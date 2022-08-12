### 闫氏DP分析法

以集合角度来分析DP问题

-----------------

##### 为什么要分析DP问题

因为难，空想想不到，类比数学三位数列式计算

----------------

#### 核心思想

所有的DP问题都是求一个有限集中的最值

一般这个有限集里元素的数量是指数级别，非常多，需要用DP优化

##### DP问题一般要经过两个阶段

1. 化零为整，即**状态表示**，`f(i)`

   将一些有相似点的元素化成一个子集，用某一个状态来表示

   一把从两个角度来分析

   1. 考虑**集合**是什么
   2. 虽然`f(i)`表示一个集合，但是存的是一个数字，可能是一个整数，布尔值，浮点数，存的数和集合之间的关系称为**属性**，一般是`max/min/count/存在与否`

2. 化整为零，即**状态计算**

   先看一下`f(i)`表示的状态是什么，**将`f(i)`划分成若干个子集**分别来求

   这些子集一般要满足：（不是所有情况都必须满足）

   1. 不重复，每个元素只属于其中一个子集

      当求数量的时候，必须要保证不重复，但是要求最大值的时候，其实是可以重复的

   2. 不遗漏，所有子集的元素一定是把`f(i)`中的元素都包含了

   比如说，`f(i)`的属性是最大值，那么整个集合的最大值就是每个子集的最大值再取Max

   - **集合划分的依据**：寻找最后一个不同点

--------------

#### <a href="https://www.acwing.com/problem/content/2/">01背包问题</a>

##### 经过理解题意，发现是有限集合求最值的问题

每个物品要么选要么不选，所以总共有 2<sup>n</sup> 种选法，要在这 2<sup>n</sup> 个方案里（有限集），找到总价值最高的方案

##### 分析

暴力方法是DFS枚举所有方案，找到总价值最高的方案，指数级别

用DP优化

1. 化零为整，即状态表示，`f(i, j)`，`i j `每一个维度表示一个条件，

   ##### 背包问题一般是选择问题，其第一维`i`表示只考虑前`i`个物品，后面几维表示限制

   1. 集合是什么，`f(i, j)`表示的集合是什么

      **集合的描述：所有满足条件一，条件二的元素的集合**，条件一条件二一般对应状态表示`f(i, j)`的每一维

      这里集合的表示：只考虑前`i`个物品，且总体积不超过`j`的选法的集合

      集合里的方案都是合法方案

      **最终答案就是`f(N, V)`的值**

   2. 属性，`f(i, j )`存的值和集合之间是什么关系

      存的值一般是看问题问的是什么

      这里属性是：集合当中每个方案的最大价值，Max

2. 化整为零，即状态计算

   划分集合`f(i, j)`，找最后一个不同点，这里最后一个不同点就是**选最后一个物品的方法（选与不选）**

   这里依据选与不选第`i`个物品，分成两个子集：（满足了不重复，不遗漏的原则）

   ##### 通过分别求这两个子集里方案的最大值，之后对两个最大值取Max，即`f(i, j)`的值

   - 所有不选第`i`个物品的方案

     该集合为在`1~i-1`物品里选，且总体积 <= `j`的方案的集合

     发现该集合就是`f(i - 1, j)`

     要求该子集中方案的最大值，也就是`f(i - 1, j)`的值

   - 所有选第`i`个物品的方案

     所有方案都是随便选 + `i`物品

     所有方案划分成：变化的部分 + 不变的部分

     - 对于不变的部分，总价值是`Wi`，对最大值没有影响
     - 对于变化的部分，在`1 ~ i-1`物品里选，总体积 <= `j - Vi`

     那么该子集中方案的最大值，也就是`f(i - 1, j - Vi)` + Wi

   最终得到`f(i, j)`的公式
$$
   f(i, j) = max(f(i - 1, j), f(i - 1, j - Vi) + Wi)
   $$
   
   ##### 注意：当`j < Vi`时，第二个子集不可能有物品`i`，那么第二个子集是空的，所以实际计算时需要特判一下

##### 分析后朴素版代码：

```java
import java.util.*;

public class Main {

    static int N = 1010;
    static int[] v = new int[N]; // 每件物体的体积
    static int[] w = new int[N]; // 每件物体的价值
    static int[][] f = new int[N][N]; // 状态

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        for (int i = 1; i <= n; i ++) {
            v[i] = sc.nextInt();
            w[i] = sc.nextInt();
        }

        for (int i = 1; i <= n; i ++) {
            for (int j = 0; j <= m; j ++) {
                f[i][j] = f[i - 1][j]; // 不选择 i 物品的方案集合最大值
                if (j >= v[i]) { // 选择 i 物品的方案集合最大值
                    f[i][j] = Math.max(f[i][j], f[i - 1][j - v[i]] + w[i]);
                }
            }
        }

        System.out.println(f[n][m]);
    }
}
```



##### 优化（DP问题的所有优化都是对代码进行等价变形）

01背包问题时间上不能优化了，只能对空间进行优化

上述分析得到方程
$$
f(i, j) = max(f(i - 1, j), f(i - 1, j - Vi) + Wi)
$$
显然，对于第一维在计算`i`层时只会用到`i - 1`层

则计算时可以使用滚动数组，每一次只保留上一层的结果，之前的结果就删掉

进一步发现，对于第二维，要么用到`j`，要么用`j - Vi`

可以把它优化成一个数组

具体循环时可以从大到小循环`for (int j = V, j >= Vi, j --)`

###### 为什么从大到小循环就可以满足等式呢

##### 优化版代码

```java
import java.util.*;

public class Main {

    static int N = 1010;
    static int[] v = new int[N];
    static int[] w = new int[N];
    static int[] f = new int[N];

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        for (int i = 1; i <= n; i ++) {
            v[i] = sc.nextInt();
            w[i] = sc.nextInt();
        }

        for (int i = 1; i <= n; i ++) {
            for (int j = m; j >= v[i]; j --) {
                f[j] = Math.max(f[j], f[j - v[i]] + w[i]);
            }
        }

        System.out.println(f[m]);
    }
}
```

--------------------

#### <a href="https://www.acwing.com/problem/content/3/)">完全背包问题</a>

- ##### 状态表示，化零为整

  - 集合：所有只从前`i`个物品里选，总体积不超过`j`的方案的集合，`f（i, j）`
  - 属性：集合里所有方案的总价值的最大值

- ##### 状态计算，化整为零

  - 划分子集

    通过选`i`物品的个数进行划分，

    第一个子集选零个`i`物品，第二个子集选一个`i`物品，依次类推，知道选出的体积大于集合限制体积为止

    不重不漏

  - 计算每一个子集的值

    从定义出发，对于第一个子集，因为包含于`f(i, j)`集合，所以一定是在`1 ~ i`物品里选，且总体积不超过`j`，又因为第一个子集选零个`i`物品，即不选`i`物品

    所以对于第一个子集，相当于在`1 ~ i-1`物品里选，且总体积不超过`j`，即`f(i - 1, j )`

    ###### 对于剩下的子集，都是类似上述的分析过程，这里不失一般性，考虑其中的某一个

    例如考虑包含了`k`个`i`物品的方案的子集，该子集中的方案都是包含了`k`个`i`物品，且其他随便选的方案，要求该子集的最大值，也即是这些方案的最大值

    这里先把所有方案都分成两部分：随便选部分和固定部分（选`k`个`i`物品部分），因此要让方案的值最大，就要让随便选部分最大

    从定义出发，由于该子集在`f(i, j)`里，且有固定部分，所以对于随便选部分即是在`1 ~ i-1`物品里选，且总体积不超过`j - kVi`，最大值是`f(i - 1, j - kVi)`

    对于包含了`k`个`i`物品的子集，其值为`f(i - 1, j - kVi)` + `kWi`

  - 计算`f(i, j)`的值

    所有子集的值取`Max`
    $$
    f(i, j) = max(f(i - 1, j), f(i - 1, j - Vi) + Wi, f(i - 1, j - 2Vi) + 2Wi, ...)
    $$
    将上述公式进行简化：

    首先将式子里的`j`替换成`j - Vi`
    $$
    f(i, j - Vi) = max(f(i - 1, j - Vi), f(i - 1, j - 2Vi) + Wi, f(i - 1, j - 3Vi) + 2Wi, ...)
    $$
    可以发现一式和二式之间有相似的项，且相似的项之间相差`Wi`

    则一式的最大值就是二式的最大值加上`Wi`，而二式的最大值是`f(i, j - Vi)`

    综上所述，最终公式简化为：
    $$
    f(i, j) = max(f(i - 1, j), f(i, j - Vi) + Wi)
    $$

##### 朴素代码：

```java
import java.util.*;

public class Main {

    static int N = 1010;
    static int[] v = new int[N];
    static int[] w = new int[N];
    static int[][] f = new int[N][N];

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        for (int i = 1; i <= n; i ++) {
            v[i] = sc.nextInt();
            w[i] = sc.nextInt();
        }

        for (int i = 1; i <= n; i ++) {
            for (int j = 0; j <= m; j ++) {
                f[i][j] = f[i - 1][j];
                if (j >= v[i]) {
                    f[i][j] = Math.max(f[i][j], f[i][j - v[i]] + w[i]);
                }
            }
        }

        System.out.println(f[n][m]);
    }
}
```

##### 优化代码（空间优化，代码等价变换）

```java
import java.util.*;

public class Main {

    static int N = 1010;
    static int[] v = new int[N];
    static int[] w = new int[N];
    static int[] f = new int[N];

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        for (int i = 1; i <= n; i ++) {
            v[i] = sc.nextInt();
            w[i] = sc.nextInt();
        }

        for (int i = 1; i <= n; i ++) {
            for (int j = v[i]; j <= m; j ++) {
                f[j] = Math.max(f[j], f[j - v[i]] + w[i]);
            }
        }

        System.out.println(f[m]);
    }
}
```

