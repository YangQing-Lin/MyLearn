#### <a href="https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/">包含min函数的栈</a>

-----------

```java
class MinStack {

    Deque<Integer> stk;
    Deque<Integer> min_stk;

    public MinStack() {
        stk = new ArrayDeque<>();
        min_stk = new ArrayDeque<>();
        min_stk.push(Integer.MAX_VALUE);
    }

    public void push(int x) {
        stk.push(x);
        min_stk.push(Math.min(x, min_stk.peek()));
    }

    public void pop() {
        stk.pop();
        min_stk.pop();
    }

    public int top() {
        return stk.peek();
    }

    public int min() {
        return min_stk.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

