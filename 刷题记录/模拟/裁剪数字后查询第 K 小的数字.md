#### <a href="https://leetcode.cn/problems/query-kth-smallest-trimmed-number/">裁剪数字后查询第 K 小的数字</a>

------------

##### 逻辑思路

- 存下每个字符串和其对应的下标

  小技巧

  - 构建一个 idx 存储对应下标，构建一个 String 字符串 tem 数组存储对应的字符串

- 依次处理询问

  - 重写 Arrays.sort 方法对 idx 进行排序
    - 排序方法为：按照 idx 存储的下标对应的在 tem 的字符串字典序排列 idx 里下标的顺序
    - 如果字典序一样，按照下标大小排序

- 对 idx 排序之后，就能找到询问的第 k 个数的下标

---------------------

```java
class Solution {
    public int[] smallestTrimmedNumbers(String[] nums, int[][] queries) {
        int n = nums.length, m = queries.length;
        
        // 存下下标及其对应的字符串
        Integer[] idx = new Integer[n];
        String[] tem = new String[n];
        for (int i = 0; i < n; i ++) {
            idx[i] = i;
            tem[i] = nums[i];
        }
        int[] res = new int[m];
        for (int i = 0; i < m; i ++) {
            int k = queries[i][0], trim = queries[i][1];
            
            // 重写 Arrays.sort 方法
            Arrays.sort(idx, new Comparator<Integer>() {
                @Override
                public int compare(Integer o1, Integer o2) {
                    for (int j = nums[0].length() - trim; j < nums[0].length(); j ++) {
                        if (tem[o1].charAt(j) > tem[o2].charAt(j)) {
                            return 1;
                        }
                        if (tem[o1].charAt(j) < tem[o2].charAt(j)) {
                            return -1;
                        }
                    }
                    return o1 - o2;
                }
            });
            res[i] = idx[k - 1];
        }
        return  res;
    }
}
```

