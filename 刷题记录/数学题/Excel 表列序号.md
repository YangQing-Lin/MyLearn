#### <a href="https://leetcode.cn/problems/excel-sheet-column-number/">Excel 表列序号</a>

-------

```java
class Solution {
    public int titleToNumber(String ct) {
        int res = 0;
        for (int i = 0; i < ct.length(); i ++) {
            res = res * 26 + (ct.charAt(i) - 'A' + 1);
        }
        return res;
    }
}
```

