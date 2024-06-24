# Day 17

## 654. 最大二叉树

题目不难，按题目要求，找到最大元素后递归构建即可。

```Java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return constructor(nums, 0, nums.length);
    }

    private TreeNode constructor(int[] nums, int left, int right) {
        // 没有元素了
        if (right <= left) {
            return null;
        }
        // 找到最大值
        int maxIndex = left;
        for (int i = left + 1; i < right; i++) {
            if (nums[i] > nums[maxIndex]) {
                maxIndex = i;
            }
        }
        // 按要求递归构建最大二叉树
        TreeNode root = new TreeNode(nums[maxIndex]);
        root.left = constructor(nums, left, maxIndex);
        root.right = constructor(nums, maxIndex + 1, right);
        return root;
    }
}
```

## 617. 合并二叉树

可以直接在root1的基础上合并，也可以new一个新节点来储存结果（更清晰）

```Java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) {
            return root2;
        }
        if (root2 == null) {
            return root1;
        }
        // 均非空，开始合并
        TreeNode root = new TreeNode(root1.val + root2.val);
        root.left = mergeTrees(root1.left, root2.left);
        root.right = mergeTrees(root1.right, root2.right);
        return root;
    }
}
```

## 700. 二叉搜索树中的搜索

利用好二叉搜索树的顺序性质即可，简单题。

```Java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) {
            return null;
        }
        if (root.val == val) {
            return root;
        } else if (root.val > val) {
            return searchBST(root.left, val);
        } else {
            return searchBST(root.right, val);
        }
    }
}
```

## 98. 验证二叉搜索树

二刷时，看一下其它的方法

```Java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    // 同时传入这个节点，要满足bst所需要的开区间范围
    public boolean isValidBST(TreeNode node, long lower, long upper) {
        if (node == null) {
            return true;
        }
        if (node.val <= lower || node.val >= upper) {
            return false;
        }
        return isValidBST(node.left, lower, node.val) && isValidBST(node.right, node.val, upper);
    }
}
```



# 
