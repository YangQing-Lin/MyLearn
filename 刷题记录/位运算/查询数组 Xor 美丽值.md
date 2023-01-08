#### <a href="https://leetcode.cn/problems/find-xor-beauty-of-array/">查询数组 Xor 美丽值</a>

-----------------

对于每一个三元组

(i, j, k) = (j, i, k) 相同，他们异或得0，可以不考虑

剩下 (a, a, b) 形式的三元组。此时可以化为二元组 (a, b) = nums[a] \& nums[b]

此时有 (a, b) = (b, a)

因此只剩下 (i, i) 形式的二元组， 即为 nums[i]

也就是把所有数异或起来即可

```java
class Solution {
    public int xorBeauty(int[] nums) {
        int res = 0;
        for (int i = 0; i < nums.length; i ++) {
            res ^= nums[i];
        }
        return res;
    }
}
```

