#### <a href="https://leetcode.cn/problems/rectangle-area-ii/">矩形面积 II</a>

--------

##### 参考题解：[【宫水三叶】扫描线模板题 - 矩形面积 II - 力扣（LeetCode）](https://leetcode.cn/problems/rectangle-area-ii/solution/gong-shui-san-xie-by-ac_oier-9r36/)

```java
class Solution {
    public int rectangleArea(int[][] rs) {
        // 定义x线段
        List<Integer> list = new ArrayList<>();
        for (int[] item : rs) {
            list.add(item[0]);
            list.add(item[2]);
        }
        Collections.sort(list);

        // 遍历每两个x线段之间的矩形
        long res = 0;
        int MOD = (int) 1e9 + 7;
        for (int i = 1; i < list.size(); i ++) {
            int a = list.get(i - 1), b = list.get(i), len = b - a; // 定义矩形的宽
            if (len == 0) continue; // 特判特殊情况：不存在的矩形

            // 找到对当前矩形有贡献的y值
            List<int[]> lines = new ArrayList<>();
            for (int[] arr : rs) {
                if (arr[0] <= a && arr[2] >= b) {
                    lines.add(new int[]{arr[1], arr[3]});
                }
            }
            lines.sort((o1, o2) -> {
                return o1[0] != o2[0] ? o1[0] - o2[0] : o1[1] - o2[1];
            });

            // 计算当前矩形的长
            int tot = 0, left = -1, right = -1;
            for (int[] temp : lines) {
                if (right < temp[0]) {
                    tot += right - left;
                    left = temp[0];
                    right = temp[1];
                } else if (right < temp[1]){
                    right = temp[1];
                }
            }
            tot += right - left; // 加上最后一个区间的长
            res += (long) tot * len; // 答案加上当前矩形的面积：长乘以宽
            res %= MOD;
        }
        return (int) res;
    }
}
```

