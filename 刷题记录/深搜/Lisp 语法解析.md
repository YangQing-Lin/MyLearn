#### <a href="https://leetcode.cn/problems/parse-lisp-expression/">Lisp 语法解析</a>

-----------------------

##### 参考[宫水三叶的题解](https://leetcode.cn/problems/parse-lisp-expression/solution/by-ac_oier-i7w1/)思路：

> 设计函数 int dfs(int l, int r, Map<String, Integer> map) 函数，代表计算 s[l...r] 的结果，答案为 dfs(0,n-1,map)，其中 nn 为字符串的长度。
>
> 根据传入的 [l,r] 是否为表达式分情况讨论：
>
> 若 s[l] = (，说明是表达式，此时从 ll 开始往后取，取到空格为止（假设位置为 idx），从而得到操作 op（其中 op 为 let、add 或 mult 三者之一），此时 op 对应参数为 [idx+1,r−1]，也就是需要跳过位置 rr（即跳过 op 对应的 ) 符号）；
>
> 再根据 op 为何种操作进一步处理，我们设计一个「传入左端点找右端点」的函数 int getRight(int left, int end)，含义为从 left 出发，一直往右找（不超过 end），直到取得合法的「子表达式」或「对应的值」。
>
> ```java
> // 对于 getRight 函数作用，给大家举个 🌰 理解吧，其实就是从 left 出发，找到合法的「子表达式」或「值」为止
> 
> // (let x 2 (mult x (let x 3 y 4 (add x y))))
> //          a                               b
> // 传入 a 返回 b，代表 [a, b) 表达式为 (mult x (let x 3 y 4 (add x y)))
> 
> // (let x 2 (mult x (let x 3 y 4 (add x y))))
> //      ab
> // 传入 a 返回 b，代表 [a, b) 表达式为 x
> ```
>
> 否则 s[l...r]s[l...r] 不是表达式，通过判断 s[l...r]s[l...r] 是否有被哈希表记录来得知结果：若在哈希表中有记录，结果为哈希表中的映射值，否则结果为本身所代表的数值。
>
> 需要注意，根据「作用域」的定义，我们不能使用全局哈希表，而要在每一层递归时，传入 clone 出来的 map。
>

-----------------

###### java

```java
class Solution {
    char[] cs;
    String s;
    public int evaluate(String _s) {
        s = _s;
        cs = s.toCharArray();
        return dfs(0, cs.length - 1, new HashMap<>());
    }
    int dfs(int l, int r, Map<String, Integer> map) {
        if (cs[l] == '(') {
            int idx = l;
            while (cs[idx] != ' ') idx++;
            String op = s.substring(l + 1, idx);
            r--; // 判别为 "(let"、"(add" 或 "(mult" 后，跳过对应的 ")"
            if (op.equals("let")) {
                for (int i = idx + 1; i <= r; ) {
                    int j = getRight(i, r);
                    String key = s.substring(i, j);
                    if (j >= r) return dfs(i, j - 1, new HashMap<>(map));
                    j++; i = j;
                    j = getRight(i, r);
                    int value = dfs(i, j - 1, new HashMap<>(map));
                    map.put(key, value);
                    i = j + 1;
                }
                return -1; // never
            } else if (op.equals("add")) {
                int j = getRight(idx + 1, r);
                int a = dfs(idx + 1, j - 1, new HashMap<>(map)), b = dfs(j + 1, r, new HashMap<>(map));
                return a + b;
            } else {
                int j = getRight(idx + 1, r);
                int a = dfs(idx + 1, j - 1, new HashMap<>(map)), b = dfs(j + 1, r, new HashMap<>(map));
                return a * b;
            }
        } else {
            String cur = s.substring(l, r + 1);
            if (map.containsKey(cur)) return map.get(cur);
            return Integer.parseInt(cur);
        }
    }
    int getRight(int left, int end) {
        int right = left, score = 0;
        while (right <= end) {
            if (cs[right] == '(') {
                score++; right++;
            } else if (cs[right] == ')') {
                score--; right++;
            } else if (cs[right] == ' ') {
                if (score == 0) break;
                right++;
            } else {
                right++;
            }
        }
        return right;
    }
}
```

