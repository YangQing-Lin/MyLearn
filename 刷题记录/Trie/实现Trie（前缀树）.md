#### <a href="https://leetcode.cn/problems/implement-trie-prefix-tree/">实现Trie（前缀树）</a>

-------------------

```java
class Trie {

    private Trie[] son;
    private boolean isEnd;

    public Trie() {
        son = new Trie[26];
       isEnd = false;
    }

    public void insert(String word) {
        Trie node = this;
        for (int i = 0; i < word.length(); i ++) {
            int u = word.charAt(i) - 'a';
            if (node.son[u] == null) {
                node.son[u] = new Trie();
            }
            node = node.son[u];
        }
        node.isEnd = true;
    }

    public boolean search(String word) {
        Trie node = searchPrefix(word);
        return node != null && node.isEnd; 
    }

    public boolean startsWith(String prefix) {
        return searchPrefix(prefix) != null;
    }

    public Trie searchPrefix(String prefix) {
        Trie node = this;
        for (int i = 0; i < prefix.length(); i ++) {
            int u = prefix.charAt(i) - 'a';
            if (node.son[u] == null) {
                return null;
            }
            node = node.son[u];
        }
        return node;
    }
}
```

