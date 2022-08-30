#### <a href="https://leetcode.cn/problems/maximum-binary-tree-ii/">最大二叉树 II</a>

-----------

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode insertIntoMaxTree(TreeNode root, int val) {
        TreeNode node = new TreeNode(val);
        TreeNode prev = null;
        TreeNode cur = root;
        while (cur != null && cur.val > val) {
            prev = cur;
            cur = cur.right;
        }

        if (prev == null) {
            node.left = root;
            return node;
        } else {
            node.left = cur;
            prev.right = node;
            return root;
        }
    }
}
```

