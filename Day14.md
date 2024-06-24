# Day 14

在递归中，广义的“前后序遍历”，可以理解为，左节点的结果，中间节点的结果，右节点的结果。

## 226. 翻转二叉树

前后序遍历都可以
中序不行，因为先左孩子交换孩子，再根交换孩子（做完后，右孩子已经变成了原来的左孩子），再右孩子交换孩子（此时其实是对原来的左孩子做交换）。

前序遍历

```Java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return root;
        }
        // 先交换
        TreeNode tmp = root.right;
        root.right = root.left;
        root.left = tmp;
        // 再翻转
        root.left = invertTree(root.left);
        root.right = invertTree(root.right);
        return root;
    }
}
```

后序遍历

```Java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root==null) {
            return null;
        }
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.right = left;
        root.left = right;
        return root;
    }
}
```

## 101. 对称的二叉树

对称，那就是左树的左边，和右树的右边比，左树的右边和右树的左边比。简单来说，就是外边比+里边比！

```Java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        // 一个树是对称二叉树，实际上就是要求左右子树对称
        return compare(root.left, root.right);
    }
    public boolean compare(TreeNode left, TreeNode right) {
        // 确定终止条件
        if (left != null && right == null) {
            return false;
        }
        if (left == null && right != null) {
            return false;
        }
        if (left == null && right == null){
            return true;
        }
        // 关键，并不是相等时返回true，而是不相等时返回false
        if (left.val != right.val) {
            return false;
        }
        boolean outFlag = compare(left.left, right.right);
        boolean inFlag = compare(left.right, right.left);
        return inFlag && outFlag;
    }
}
```



在做深度有关的题目之前，需知：

- 二叉树节点的深度：指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）
- 二叉树节点的高度：指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）

leetcode中强调的深度和高度一般是按照节点来计算的。

求深度可以从上到下去查，所以需要前序遍历（中左右），而高度只能从下到上去查，所以只能后序遍历（左右中）

## 104. 二叉树的最大深度

二叉树的最大深度，就是根节点的高度。

本质上，前序求的是深度，后序求的是高度。

后序遍历：

左节点的结果，右节点的结果 =》中间节点的结果

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
      	// 这里的Depth换成Height更好理解
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        return Math.max(leftDepth, rightDepth) + 1;
    }
}
```

前序遍历：

```Java
class Solution {
    int maxDepth = 0;
    public int maxDepth(TreeNode root) {
        getDepth(root, 1);
        return maxDepth;
    }
    public void getDepth(TreeNode node, int depth) {
        if (node == null) {
            return;
        }
        // 中
        maxDepth = Math.max(maxDepth, depth);
        // 左
        if (node.left != null) {
            getDepth(node.left, depth + 1);
        }
        // 右
        if (node.right != null) {
            getDepth(node.right, depth + 1);
        }
    }
}
```

或者用层序遍历，每层结束了 + 1深度。

## 111. 二叉树的最小深度

题目中说的是：**最小深度是从根节点到最近叶子节点的最短路径上的节点数量。**

注意是左右孩子都为空的节点才是叶子节点！

<img src="/Users/anhejia/Desktop/截屏2024-06-21 16.56.43.png" alt="截屏2024-06-21 16.56.43" style="zoom:30%;" />



采用后序遍历，左最小，右最小，总最小。

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        // 左右子树有一个为空的情况
        if (root.left == null) {
            return minDepth(root.right) + 1;
        }
        if (root.right == null) {
            return minDepth(root.left) + 1;
        }
        // 左右子树都不为空的情况
        int leftDepth = minDepth(root.left);
        int rightDepth = minDepth(root.right);
        return Math.min(leftDepth, rightDepth) + 1;
    }
}
```

