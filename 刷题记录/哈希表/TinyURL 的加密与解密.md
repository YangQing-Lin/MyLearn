#### <a href="https://leetcode.cn/problems/encode-and-decode-tinyurl/submissions/">TinyURL 的加密与解密</a>

----------

###### c++

```c++
class Solution {
private:
    unordered_map<int, string> map;
    int id = 0;

public:

    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        id ++;
        map[id] = longUrl; // 映射
        return "http://tinyurl.com/" + to_string(id);
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        int p = shortUrl.rfind('/') + 1; // 从后找 '/' 并返回下标，此时再加一，即是 id 的起始坐标
        int key = stoi(shortUrl.substr(p, shortUrl.size() - p)); // 把字符串 id 转化为 int 作为key 找出对应的value（longUrl）
        return map[key];
    }
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```

###### java

```java
public class Codec {
    Map<Integer, String> map = new HashMap<Integer, String>();
    int id = 0;

    public String encode(String longUrl) {
        id++;
        map.put(id, longUrl); // 映射
        return "http://tinyurl.com/" + id;
    }

    public String decode(String shortUrl) {
        int p = shortUrl.lastIndexOf('/') + 1; // 从后找 '/' 并返回下标，此时再加一，即是 id 的起始坐标
        int key = Integer.parseInt(shortUrl.substring(p)); // 把字符串 id 转化为 int 作为key 找出对应的value（longUrl）
        return map.get(key);
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```

