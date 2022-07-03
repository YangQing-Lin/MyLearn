#### <a href="https://leetcode.cn/problems/next-greater-element-iii/">下一个更大元素 III</a>

------------------------

###### java

```java
class Solution {
    public int nextGreaterElement(int n) {
        char[] cs = Integer.toString(n).toCharArray();

        int i = cs.length - 2;
        while (i >= 0 && cs[i] >= cs[i + 1]) {
            i --;
        }

        if (i < 0) {
            return -1;
        }

        int j = cs.length - 1;
        while (j > i && cs[j] <= cs[i]) {
            j --;
        }
        swap(cs, i, j);
        reverse(cs, i + 1, cs.length - 1);

        long ans = Long.parseLong(new String(cs)); // 把字符数组转换为字符串，再转换为long型整数
        return ans > Integer.MAX_VALUE ? -1 : (int) ans;
    }

    public void swap(char[] cs, int left, int right) {
        char tem = cs[left];
        cs[left] = cs[right];
        cs[right] = tem;
    }

    public void reverse(char[] cs, int left, int right) {
        int i = left, j = right;
        while (i < j) {
            swap(cs, i, j);
            i ++;
            j --;
        }
    }
}
```

