#### <a href="https://leetcode.cn/problems/my-calendar-iii/">我的日程安排 III</a>

--------------

###### c++

```c++
class MyCalendarThree {
public:
    MyCalendarThree() {
        
    }
    
    int book(int start, int end) {
        int ans = 0;
        int maxBook = 0;
        cnt[start]++;
        cnt[end]--;
        for (auto &[_, freq] : cnt) {
            maxBook += freq;
            ans = max(maxBook, ans);
        }
        return ans;
    }
private:
    map<int, int> cnt;
};
```

