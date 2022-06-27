#### <a href="https://leetcode.cn/problems/longest-uncommon-subsequence-ii/submissions/">最长特殊序列 II</a>

---------

###### c++

```c++
class Solution {
public:
    unordered_map<string, int> map;

    bool isZixl(string s, string a) {
        int i = 0, j = 0;
        while (i < s.size() && j < a.size()) {
            if (s[i] == a[j]) {
                j ++;
            }
            i ++;
        }
        
        if (j == a.size()) {
            return true;
        }

        return false;
        
    }

    int findLUSlength(vector<string>& strs) {
        int n = strs.size();
        for (int i = 0; i < n; i ++) {
            for (int j = 0; j < n; j ++) {
                if (isZixl(strs[i], strs[j])) {
                    map[strs[j]] ++;
                }
            }
        }

        int res = 0;
        bool flag = false;
        for (auto p : map) {
            if (p.second == 1) {
                int t = p.first.size();
                res = max(res, t);
                flag = true;
            }
        }

        if (!flag) {
            return -1;
        }

        return res;
    }
};
```

###### java

```java
class Solution {
    HashMap<String, Integer> hashmap = new HashMap<String, Integer>();

    public boolean isZixl(String s, String a) {
        int i = 0, j = 0;
        while (i < s.length() && j < a.length()) {
            if (s.charAt(i) == a.charAt(j)) {
                j ++;
            }
            i ++;
        }
        
        if (j == a.length()) {
            return true;
        }

        return false;
    }

    public int findLUSlength(String[] strs) {
        int n = strs.length;
        for (int i = 0; i < n; i ++) {
            for (int j = 0; j < n; j ++) {
                if (isZixl(strs[i], strs[j])) {
                    if (!hashmap.containsKey(strs[j])) {
                        hashmap.put(strs[j], 1);
                        continue;
                    }
                    if (hashmap.containsKey(strs[j])) {
                        hashmap.put(strs[j], 2);
                    }
                }
            }
        }

        for (var entry : hashmap.entrySet()) {
            String key = entry.getKey();
            int value = entry.getValue();
            System.out.println(key);
            System.out.println(value);
        }

        int res = 0;
        boolean flag = false;
        for (var entry : hashmap.entrySet()) {
            String key = entry.getKey();
            int value = entry.getValue();
            if (value == 1) {
                int t = key.length();
                res = Math.max(res, t);
                flag = true;
            }
        }

        if (!flag) {
            return -1;
        }

        return res;
    }
}
```

