#### <a href="https://leetcode.cn/problems/defanging-an-ip-address/submissions/">IP 地址无效化</a>

----------

###### c++

```c++
class Solution {
public:
    string defangIPaddr(string address) {
        string tem;
        int n = address.size();
        for (int i = 0; i < n; i ++) {
            if (address[i] == '.') {
                tem += "[.]";
                continue;
            }
            tem += address[i];
        }

        return tem;
    }
};
```

###### java

```java
class Solution {
    public String defangIPaddr(String address) {
        return address.replace(".", "[.]");
    }
}
```

