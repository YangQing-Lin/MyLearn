#### <a href="https://leetcode.cn/problems/sentence-similarity-iii/">句子相似性 III</a>

------------

```java
class Solution {
    public boolean areSentencesSimilar(String sentence1, String sentence2) {
        // 保证sentence1是较短的句子
        if (sentence1.length() > sentence2.length()) return areSentencesSimilar(sentence2, sentence1);
        
        // 去掉空格
        String[] str1 = sentence1.split(" ");
        String[] str2 = sentence2.split(" ");
        int n = str1.length, m = str2.length;
        
        // 比较前缀
        int i = 0;
        for (int k = 0; k < m && i < n; k ++) {
            if (Objects.equals(str1[i], str2[k])) i ++;
            else break;
        }

        // 比较后缀
        int j = n - 1;
        for (int k = m - 1; k >= 0 && j >= 0; k --) {
            if (str1[j].equals(str2[k])) j --;
            else break;
        }
        
        // 判断较短串是否被覆盖
        return i > j;
    }
}
```

