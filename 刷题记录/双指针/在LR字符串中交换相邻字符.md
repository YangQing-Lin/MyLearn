#### <a href="https://leetcode.cn/problems/swap-adjacent-in-lr-string/submissions/">在LR字符串中交换相邻字符</a>

-----------

```java
class Solution {
    public boolean canTransform(String st, String ed) {
        int n = st.length();
        int i = 0, j = 0;
        while (i < n && j < n) {
            while (i < n && st.charAt(i) == 'X') i ++;
            while (j < n && ed.charAt(j) == 'X') j ++;

            if (i < n && j < n) {
                if (st.charAt(i) != ed.charAt(j)) return false;

                char c = st.charAt(i);
                if (c == 'R' && i > j || c == 'L' && i < j) return false;

                i ++;
                j ++;
            }
        }

        while (i < n) {
            if (st.charAt(i) != 'X') return false;
            i ++;
        }
        while (j < n) {
            if (ed.charAt(j) != 'X') return false;
            j ++;
        }
        return true;
    }
}
```

