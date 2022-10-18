#### <a href="https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/">最大为 N 的数字组合</a>

----------------

##### 参考题解：[宫水三叶](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/solution/by-ac_oier-62ws/)

```java
class Solution {

    int[] nums;

    public int atMostNGivenDigitSet(String[] digits, int n) {
        nums = new int[digits.length];
        for (int i = 0; i < digits.length; i ++) nums[i] = Integer.parseInt(digits[i]);
        return dp(n);
    }

    public int dp(int n) {
        // 将 n 的每一位拆出来讨论
        List<Integer> list = new ArrayList<>();
        while (n > 0) {
            list.add(n % 10);
            n /= 10;
        }

        // 讨论位数与n相同的数是否合法
        int q = nums.length; int res = 0; int p = list.size();
        for (int i = p - 1, t = 1; i >= 0; i --, t ++) {
            int cur = list.get(i);

            // 在nums里找到比cur小的最大值的下标
            int l = 0, r = q - 1;
            while (l < r) {
                int mid = l + r + 1 >> 1;
                if (nums[mid] <= cur) l = mid;
                else r = mid - 1;
            }

            if (nums[r] > cur) break;
            else if (nums[r] == cur) { // 前面的位上的数都和n相等
                res += r * (int) Math.pow(q, (p - t));
                if (i == 0) res ++;
            } else if (nums[r] < cur) {
                res += (r + 1) * (int) Math.pow(q, (p - t));
                break;
            }
        }
        
        // 讨论位数比n小的数是否合法
        for (int i = 1, last = 1; i < p; i ++) {
            int cur = last * q;
            res += cur;
            last = cur;
        }
        return res;
    }
}
```

