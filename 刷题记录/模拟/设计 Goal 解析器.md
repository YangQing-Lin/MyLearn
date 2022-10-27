#### <a href="https://leetcode.cn/problems/goal-parser-interpretation/">设计 Goal 解析器</a>

-------------

```java
class Solution {
    public String interpret(String command) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < command.length(); i ++) {
            if (command.charAt(i) == 'G') {
                sb.append('G');
            } else if (command.charAt(i) == '(') {
                String t = "";
                int j = i;
                while (j < command.length() && command.charAt(j) != ')') {
                    t += command.charAt(j);
                    j ++;
                }
                t += command.charAt(j);
                if ("()".equals(t)) sb.append('o');
                else sb.append("al");
                i = j;
            }
        }
        return sb.toString();
    }
}
```

