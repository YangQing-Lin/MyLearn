#### <a href="https://leetcode.cn/problems/strong-password-checker-ii/">强密码检验器 II</a>

----------------

```java
class Solution {
    public boolean strongPasswordCheckerII(String password) {
        int n = password.length();
        if (n < 8) return false;
        Set<Character> set = new HashSet<Character>(){{
            add('!');
            add('@');
            add('#');
            add('$');
            add('%');
            add('^');
            add('&');
            add('*');
            add('(');
            add(')');
            add('-');
            add('+');
        }};

        boolean flag1 = false, flag2 = false, flag3 = false, flag4 = false;
        for (int i = 0; i < password.length(); i ++) {
            char c = password.charAt(i);
            if (c >= 'a' && c <= 'z') flag1 = true;
            if (c >= 'A' && c <= 'Z') flag2 = true;
            if (Character.isDigit(c)) flag3 = true;
            if (set.contains(c)) flag4 = true;
            if (i != 0 && password.charAt(i) == password.charAt(i - 1)) return false;
        }
        return flag1 && flag2 && flag3 && flag4;
    }
}
```

