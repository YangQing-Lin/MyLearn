#### <a href="https://leetcode.cn/problems/next-greater-element-i/">下一个更大元素 I</a>

------------

```java
class Solution {
    public int[] nextGreaterElement(int[] n1, int[] n2) {
        Deque<Integer> stk = new ArrayDeque<>();
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = n2.length - 1; i >= 0; i --) {
            while (!stk.isEmpty() && stk.peek() <= n2[i]) stk.pop();
            if (!stk.isEmpty()) {
                map.put(n2[i], stk.peek());
            } else {
                map.put(n2[i], -1);
            }
            stk.push(n2[i]);
        }

        int[] res = new int[n1.length];
        for (int i = 0; i < n1.length; i ++) {
            res[i] = map.get(n1[i]);
        }
        return res;
    }
}
```

