#### <a href="https://leetcode.cn/problems/the-k-weakest-rows-in-a-matrix/">矩阵中战斗力最弱的 K 行</a>

------------

```java
class Solution {
    public int[] kWeakestRows(int[][] mat, int k) {
        int n = mat.length, m = mat[0].length;

        List<int[]> list = new ArrayList<>();
        for (int i = 0; i < n; i ++) {
            int l = 0, r = m - 1;
            while (l < r) {
                int mid = l + 1 + (r - l) / 2;
                if (mat[i][mid] == 1) l = mid;
                else r = mid - 1;
            }
            int t = mat[i][r] == 1 ? r + 1 : -1;
            list.add(new int[]{t, i});
        }

        PriorityQueue<int[]> heap = new PriorityQueue<>(new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                if (o1[0] != o2[0]) {
                    return o1[0] - o2[0];
                }
                return o1[1] - o2[1];
            }
        });
        for (int i = 0; i < list.size(); i ++) {
            heap.offer(list.get(i));
        }
        
        int[] res = new int[k];
        for (int i = 0; i < k; i ++) {
            res[i] = heap.poll()[1];
        }
        return res;
    }
}
```

