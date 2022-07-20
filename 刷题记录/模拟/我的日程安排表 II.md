#### <a href="https://leetcode.cn/problems/my-calendar-ii/">我的日程安排表 II</a>

---------------------

```java
class MyCalendarTwo {

    private List<int[]> list1 = new ArrayList<>();
    private List<int[]> list2 = new ArrayList<>();

    public MyCalendarTwo() {

    }

    public boolean book(int start, int end) {
        for (int[] a : list1) {
            if (start < a[1] && end > a[0]) {
                return false;
            } 
        }
        
        for (int[] b : list2) {
            if (start < b[1] && end > b[0]) {
                int x = Math.max(start, b[0]);
                int y = Math.min(end, b[1]);
                int[] t = {x, y};
                list1.add(t);
            }
        }
        int[] tem = {start, end};
        list2.add(tem);
        
        return true;
    }
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo obj = new MyCalendarTwo();
 * boolean param_1 = obj.book(start,end);
 */
```

