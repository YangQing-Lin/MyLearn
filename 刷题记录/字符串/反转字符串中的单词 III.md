#### <a href="https://leetcode.cn/problems/reverse-words-in-a-string-iii/">反转字符串中的单词 III</a>

--------------

```java
class Solution {
    public String reverseWords(String s) {
        StringBuilder sb = new StringBuilder(s);
        sb.reverse();
        String tem = sb.toString();
        String[] arr = tem.split(" ");

        for (int i = 0, j = arr.length - 1; i < j; i ++, j --) {
            String t = arr[i];
            arr[i] = arr[j];
            arr[j] = t;
        }

        StringBuilder res = new StringBuilder();
        for (int i = 0; i < arr.length; i ++) {
            res.append(arr[i]);
            if (i == arr.length - 1) continue;
            res.append(" ");
        }
        return res.toString();
    }
}
```

