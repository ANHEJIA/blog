# Day 16

## 513. 找树左下角的值

用递归的话就就一直向左遍历， 不一定就是最左下角。

所以，应该是返回层序遍历的最后一层的第一个节点值。

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> que = new LinkedList<>();
        que.offer(root);
        int res = 0;
        while (!que.isEmpty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = que.poll();
                // 记录每层第一个节点的值
                if (i == 0) {
                    res = cur.val;
                }
                if (cur.left != null) {
                    que.offer(cur.left);
                }
                if (cur.right != null) {
                    que.offer(cur.right);
                }
            }
        }
        return res; // 最后退出时的值就是左下角的值
    }
}
```

## 112. 路径总和

二刷时，补一下其他方法

```Java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        // 节点为空，退出
        if (root == null) {
            return false;
        }
        // 到达叶子节点，检查「此刻要求的」targetSum
        if (root.left == null && root.right == null) {
            return root.val == targetSum;
        }
        // 层层外包
        return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum - root.val);
    }
}
```

## 113. 路径总和ll

二刷时补充

## 106. 从中序和后序遍历序列构建二叉树

二刷时补充

## 105. 从前序和后序遍历序列构建二叉树

同剑指offer 07.重建二叉树

