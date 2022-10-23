#### <a href="https://leetcode.cn/problems/n-ary-tree-preorder-traversal/">N 叉树的前序遍历</a>

------------

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {

    List<Integer> list;

    public List<Integer> preorder(Node root) {
        list = new ArrayList<>();
        dfs(root);
        return list;
    }

    public void dfs(Node root) {
        if (root == null) return;
        
        list.add(root.val);
        List<Node> node = root.children;
        for (Node t : node) {
            dfs(t);
        }
    }
}
```

