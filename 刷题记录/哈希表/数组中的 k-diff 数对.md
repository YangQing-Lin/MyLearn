#### <a href="https://leetcode.cn/problems/k-diff-pairs-in-an-array/submissions/">数组中的 k-diff 数对</a>

------------

###### c++

```c++
class Solution {
public:
    int findPairs(vector<int>& nums, int k) {\
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int l = 0, r = l + 1;
        unordered_map<int, int> map;
        while (l < n && r < n) {
            if (abs(nums[l] - nums[r]) == k) {
                map.insert(pair<int, int>(nums[l], nums[r]));
            }

            if (r == n - 1) {
                l ++;
                r = l + 1;
                continue;
            }

            r ++;
        }

        return map.size();
    }
};
```

###### java

```java
class Solution {
    public int findPairs(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int l = 0, r = l + 1;
        Map<Integer, Integer> map = new HashMap<Integer, Integer>(); 
        while (l < n && r < n) {
            if (nums[l] - nums[r] == k || nums[r] - nums[l] == k) {
                map.put(nums[l], nums[r]);
            }

            if (r == n - 1) {
                l ++;
                r = l + 1;
                continue;
            }

            r ++;
        }

        return map.size();
    }
}
```

