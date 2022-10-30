#### <a href="https://leetcode.cn/problems/next-greater-element-iv/">下一个更大元素 IV</a>

-----------

#### TreeSet + lowerbound

将下标按值大小降序排序，利用TreeSet和lowerbound函数找到比当前下标大的第二个下标

##### java

```java
class Solution {

    TreeSet<Integer> set = new TreeSet<>();

    public int[] secondGreaterElement(int[] nums) {
        int n = nums.length;

        // 将下标降序排序
        Integer[] p = new Integer[n];
        for (int i = 0; i < n ; i ++) p[i] = i;
        Arrays.sort(p, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return nums[o2] - nums[o1];
            }
        });

        // tailset报错
        set.add(n + 10); set.add(n + 11);

        // 从前往后处理下标，即以值的降序处理
        int[] res = new int[n];
        Arrays.fill(res, -1);
        for (int i = 0; i < n; i ++) {

            // 批处理，因为nums里有重复数
            int j = i + 1;
            while (j < n && nums[p[i]] == nums[p[j]]) j ++; // i ... j - 1 之间就是重复数

            // 由于是从值大到小处理，所以值比当前数大的已经在set里了，只需要找到值大的第二个下标
            for (int k = i; k < j; k ++) {
                
                // 迭代器找到比 p[k] 大的在set里的第二个数
                Set<Integer> sub = set.tailSet(p[k]);
                Iterator<Integer> iterator = sub.iterator();
                iterator.next();
                int index = iterator.next();
                if (index < n) res[p[k]] = nums[index]; // set里有两个哨兵下标，这里需要剔除
            }

            // 将处理之后的下标加入set
            for (int k = i; k < j; k ++) set.add(p[k]);

            // 注意下标要移到处理之后的下标
            i = j - 1;
        }

        return res;
    }
}
```

##### c++

```c++
class Solution {
public:
    vector<int> secondGreaterElement(vector<int>& w) {
        int n = w.size();
        vector<int> p(n);
        for (int i = 0; i < n; i ++ ) p[i] = i;
        sort(p.begin(), p.end(), [&](int a, int b) {
            return w[a] > w[b];
        });
        
        vector<int> res(n, -1);
        
        set<int> S;
        S.insert(n + 10), S.insert(n + 11);
        
        for (int i = 0; i < n; i ++ ) {
            int j = i + 1;
            while (j < n && w[p[j]] == w[p[i]]) j ++ ;
            
            for (int k = i; k < j; k ++ ) {
                auto it = S.lower_bound(p[k]);
                ++it;
                if (*it < n) res[p[k]] = w[*it];
            }
            
            for (int k = i; k < j; k ++ ) S.insert(p[k]);
            
            i = j - 1;
        }
        
        return res;
    }
};
```

