# Day 15

## 110. 平衡二叉树

平衡二叉树是一棵二叉树，其中每个节点的左右子树的**高度差**（也称为平衡因子）最多为1。

根据定义：

一棵二叉树是平衡二叉树 =》所有子树也都是平衡二叉树

左右子树是平衡二叉树（除了根节点，都满足高度差最多为1） + 根节点的左右子树的高度差也是1 =》一棵二叉树是平衡二叉树

求高度，那就是用后序遍历。

```Java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        // 1. 根节点满足
        int diff = getHeight(root.left) - getHeight(root.right);
        if (Math.abs(diff) > 1) {
            return false;
        }
        // 2. 左右子树都是平衡二叉树（除了根节点以外的所有节点）
        return isBalanced(root.left) && isBalanced(root.right);
    }
    // 获取高度
    public int getHeight(TreeNode node) {
        if (node == null) {
            return 0;
        }
        return Math.max(getHeight(node.left), getHeight(node.right)) + 1;
    }
}
```

总的来说，这属于从顶向下的写法，不管三七二十一先都算一遍。

还有一种是自底向上的写法，可以及时止损。



## 257. 二叉树的所有路径

这道题目要求从根节点到叶子的路径，所以需要前序遍历，这样才方便让父节点指向孩子节点，找到对应的路径。

同时需要回溯，因为我们要把路径记录下来，需要回溯来回退一个路径再进入另一个路径。

```Java
class Solution {
    List<String> paths = new ArrayList<>(); // 存储结果格式
    List<Integer> path = new ArrayList<>(); // 存储当前路径
    public List<String> binaryTreePaths(TreeNode root) {
        if (root == null) {
          	return paths;
        }
      	traverse(root);
        return paths;
    }
    public void traverse(TreeNode node) {        
        path.add(node.val); // 前序
        // 1. 找到了叶子结点
        if (node.left == null && node.right == null) {
            // 处理当前路径，按题目要求格式输出
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < path.size() - 1; i++) {
                sb.append(path.get(i)).append("->");
            }
            sb.append(path.get(path.size() - 1)); // 最后一个单独处理，不用加箭头
            paths.add(sb.toString()); // 存放结果
        }
        // 2. 还不是叶子结点
        if (node.left != null) {
            traverse(node.left); // 递归
            path.remove(path.size() - 1); // 回溯
        }
        if (node.right != null) {
            traverse(node.right); // 递归
            path.remove(path.size() - 1); // 回溯
        }
    }
}
```



## 404. 左叶子之和

首先要捋清楚，什么是左叶子？这边的定义是：**节点A的左孩子不为空，且左孩子的左右孩子都为空（说明是叶子节点），那么A节点的左孩子为左叶子节点。**

**判断当前节点是不是左叶子是无法判断的，必须要通过节点的父节点来判断其左孩子是不是左叶子。**

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        // 终止条件
        if (root == null) {
            return 0;
        }
        // 无子节点，剪枝（可去）
        if (root.left == null && root.right == null) {
            return 0;
        }
        int midValue = 0; // 中
        // 判断左子节点情况
        if (root.left != null && root.left.left == null && root.left.right == null) {
            midValue = root.left.val; // 修改中间节点结果
        }
        int leftValue = sumOfLeftLeaves(root.left); // 左
        int rightValue = sumOfLeftLeaves(root.right); // 右
        return midValue + leftValue + rightValue;
    }
}
```



## 222. 完全二叉树的节点个数

暂时先用普通二叉树的通用解法解决。没有充分利用完全二叉树的性质，还能进一步优化。

```Java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return countNodes(root.left) + countNodes(root.right) + 1;
    }
}
```

