#### <a href="https://leetcode.cn/problems/max-chunks-to-make-sorted-ii/">最多能完成排序的块 II</a>

----------

##### 排序＋哈希表

##### 找元素频次相同的块

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        int n = arr.length;
        int[] sorted = Arrays.copyOf(arr, n);
        Arrays.sort(sorted);
        
        int res = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i ++) {
            int a = arr[i];
            int b = sorted[i];
            map.put(a, map.getOrDefault(a, 0) + 1);
            if (map.get(a) == 0) {
                map.remove(a);
            }
            map.put(b, map.getOrDefault(b, 0) - 1);
            if (map.get(b) == 0) {
                map.remove(b);
            }
            if (map.isEmpty()) {
                res ++;
            }
        }
        return res;
    }
}
```

-------

##### 单调栈

比较每个块的最大值

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        Deque<Integer> stack = new ArrayDeque<>();
        for (int num : arr) {
            if (stack.isEmpty() || stack.peek() <= num) {
                stack.push(num);
            } 
            else {
                int t = stack.pop();
                while (!stack.isEmpty() && stack.peek() > num) {
                    stack.pop();
                }
                stack.push(t);
            }
        }
        return stack.size();
    }
}
```

