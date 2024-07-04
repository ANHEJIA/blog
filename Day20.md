# Day 20

### **235. 二叉搜索树的最近公共祖先**

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 都在右子树
        if (p.val > root.val && q.val > root.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        // 都在左子树
        if (p.val < root.val && q.val < root.val) {
            return lowestCommonAncestor(root.left, p, q);
        }
        // 一左一右，当中的root节点即为最近公共祖先
        return root;
    }
}
```

### **701.二叉搜索树中的插入操作**

输入数据保证，新值和原始二叉搜索树中的任意节点值都不同。

所以，只要按照二叉搜索树的规则去遍历，遇到空节点就插入节点就可以了。

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        // 找到了空的位置，插入，返回（新节点的根节点就是其本身）
        if (root == null) {
            return new TreeNode(val);
        }
        // 利用搜索树的性质，层层传递本层的”根节点“
        if (root.val > val) {
            root.left = insertIntoBST(root.left, val); // 后者返回的是插入新节点后，新的root.left
        }
        if (root.val < val) {
            root.right = insertIntoBST(root.right, val); // 后者返回的是插入新节点后，新的root.right
        }
        // 返回插入后二叉搜索树的根节点
        return root; 
    }
}
```

### **450.删除二叉搜索树中的节点**

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return root;
        }
        // 删除节点逻辑，要利用好二叉搜索树的性质
        if (root.val == key) {
            if (root.left == null) { // 1. 左子树为空
                return root.right;
            } else if (root.right == null) { // 2. 右子树为空
                return root.left;
            } else { // 3. 左右子树均不为空
                // 找到右子树的最左（小）节点
                TreeNode cur = root.right;
                while (cur.left != null) {
                    cur = cur.left;
                }
                // 把左子树连过去
                cur.left = root.left;
                // 右子树的根节点称为新树的根节点，老的根节点就被删除了
                root = root.right;
                return root;
            }
        }
        // 递归删除
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
        }
        if (root.val < key) {
            root.right = deleteNode(root.right, key);
        }
        return root;
    }
}
```
