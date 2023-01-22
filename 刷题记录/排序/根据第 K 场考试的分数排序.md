#### <a href="https://leetcode.cn/problems/sort-the-students-by-their-kth-score/">根据第 K 场考试的分数排序</a>

----------------

```java
class Solution {
    public int[][] sortTheStudents(int[][] score, int k) {
        Arrays.sort(score, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o2[k] - o1[k];
            }
        });
        return score;
    }
}
```

