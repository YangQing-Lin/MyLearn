#### <a href="https://leetcode.cn/problems/shifting-letters-ii/">字母移位 II</a>

-----------

```java
class Solution {
    public String shiftingLetters(String s, int[][] shifts) {
        int n = s.length();
        int[] b = new int[n + 1];
        for (int[] st : shifts) {
            int l = st[0];
            int r = st[1];
            int w = st[2];
            if (w == 0) {
                w = -1;
            }
            b[l] += w;
            b[r + 1] -= w;
        }

        for (int i = 1; i < b.length; i ++) {
            b[i] += b[i - 1];
        }

        char[] c = s.toCharArray();
        for (int i = 0; i < n; i ++) {
            int x = c[i] - 'a';
            x = (x + b[i] % 26 + 26) % 26;
            c[i] = (char) (x + 'a');
        }
        return new String(c);
    }
}

```

