#### <a href="https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/">从上到下打印二叉树 III</a>

-------------

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) return new ArrayList<>();

        List<List<Integer>> res = new ArrayList<>();

        Deque<TreeNode> queue = new ArrayDeque<>();
        queue.addLast(root);
        int t = 1;
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            for (int i = 0; i < size; i ++) {
                TreeNode node = queue.pollFirst();
                if (node.left != null) queue.addLast(node.left);
                if (node.right != null) queue.addLast(node.right);
                list.add(node.val);
            }
            if (t ++ % 2 == 0) {
                Collections.reverse(list);
            }
            res.add(list);
        }
        return res;
    }
}
```

