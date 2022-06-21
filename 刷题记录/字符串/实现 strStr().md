#### <a href="https://leetcode.cn/problems/implement-strstr/">实现 strStr()</a>

-----------

###### c++

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size();
        int m = needle.size();
        
        if (m == 0) {
            return 0;
        }

        for (int i = 0; i < n; i ++) {
            if (haystack[i] == needle[0]) {
                string k;
                for (int j = i; j < i + m && j < n; ++ j) {
                    k += haystack[j];
                }
                if (k == needle) {
                    return i;
                }
            }
        }

        return -1;
    }
};
```

###### java

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length(), m = needle.length();
        for (int i = 0; i + m <= n; i++) {
            boolean flag = true;
            for (int j = 0; j < m; j++) { // 比较 haystack 中每一个长度为 m 的子串
                if (haystack.charAt(i + j) != needle.charAt(j)) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                return i;
            }
        }
        return -1;
    }
}
```

