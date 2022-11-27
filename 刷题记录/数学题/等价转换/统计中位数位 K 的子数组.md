#### <a href="https://leetcode.cn/problems/count-subarrays-with-median-k/">统计中位数位 K 的子数组</a>

-------------------

参考题解：[0x3f](https://leetcode.cn/problems/count-subarrays-with-median-k/solution/deng-jie-zhuan-huan-pythonjavacgo-by-end-5w11/)

##### 数学思维和等价转换

定义 $Min$ 为子数组中小于 $k$ 的元素个数，$Max$ 为子数组中大于 $k$ 的个数

**当子数组的长度为奇数时**

要使 $k$ 为子数组的中位数，则需要满足
$$
Min = Max
$$
为了方便统计，我们将 $min$ 和 $max$ 的元素根据所处 $k$ 的左右侧情况，分成两部分
$$
leftMin + rightMin = leftMax + rightMax
$$
合并同类项？
$$
+leftMin + -leftMax = +rightMax + -rightMin
$$
我们可以分别计算左边和右边

定义 $pos$ 为 $k$ 所在的下标

**先计算右边**，$[pos, n - 1]$

使用变量 $c$ 表示当前子数组的系数和，哈希表表示 $c$ 的数量

**计算左边**

同样的使用变量 $c$ 表示当前子数组的系数和，同时在答案里加上哈希表中统计的右边 $c$ 的数量

##### 当子数组的长度为偶数的时候

可以发现，上述的等价转换式子写为
$$
+leftMin + -leftMax + 1 = +rightMax + -rightMin
$$
则对于偶数的情况，只需要在计算左边的时候，在答案里加上哈希表中统计的右边 $c + 1$ 的数量

```java
class Solution {
    public int countSubarrays(int[] nums, int k) {
        int n = nums.length;
        int pos = 0;
        while (nums[pos] != k) ++ pos;

        var map = new HashMap<Integer, Integer>();
        int c = 0;
        map.put(0, 1); // 将 i=pos 的情况直接在加到哈希表里统计，这样下面的for循环中只需要判断大于和小于的情况
        for (int i = pos + 1; i < n; i ++) {
            c += nums[i] > k ? 1 : -1;
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        
        c = 0;
        int res = map.get(0) + map.getOrDefault(1, 0); // 将 i=pos 的情况直接在加到哈希表里统计，这样下面的for循环中只需要判断大于和小于的情况
        for (int i = pos - 1; i >= 0; i --) {
            c += nums[i] < k ? 1 : -1;
            res += map.getOrDefault(c, 0) + map.getOrDefault(c + 1, 0);
        }
        return res;
    }
}
```

 