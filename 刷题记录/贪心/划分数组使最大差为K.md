#### <a href="https://leetcode.cn/problems/partition-array-such-that-maximum-difference-is-k/">划分数组使最大差为K</a>

--------------

```c++
class Solution {
public:
    int partitionArray(vector<int>& nums, int k) {
        int n = nums.size();

        sort(nums.begin(), nums.end());

        int res = 0;
        for (int i = 0, j = 0; i < n; i++) {
            if (nums[i] - nums[j] > k) {
                res ++;
                j = i;
            }
        }
        
        return res + 1;
    }
};
```

