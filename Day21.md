# Day 21

## 669. 修建二叉搜索树

本题的关键，在于理解为什么完成了“修建”的动作。

```Java
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }
        // 寻找符合区间[low, high]的节点，“修剪”不符合的另一支
        if (root.val < low) {
            return trimBST(root.right, low, high);
        }
        if (root.val > high) {
            return trimBST(root.left, low, high);
        }
        // root处在区间[low, high]间
        root.left = trimBST(root.left, low, high); // 接入符合条件的左孩子
        root.right = trimBST(root.right, low, high); // 接入符合条件的右孩子
        return root;
    }
}
```

## 108. 将有序数组转换为平衡二叉搜索树

注意，别遗漏了**平衡**的要求。

本质就是寻找分割点，分割点作为当前节点，然后递归左区间和右区间。

思考一下，二叉搜索树的中序遍历就是一个有序数组。所以重构的时候也是以“中间节点”作为分割点。

如果数组长度为偶数，中间节点有两个，取哪一个？选左选右其实都可以，对应了不同的结果。

同时，本题也要思考清楚区间是左闭右闭，还是左闭右开？

1. 左闭右闭

```Java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return traversal(nums, 0, nums.length - 1);
    }
    public TreeNode traversal(int[] nums, int left, int right) {
        if (left > right) {
            return null;
        }
        int mid = (left + right) / 2; // 选了中间偏左的节点
        TreeNode root = new TreeNode(nums[mid]);
        root.left = traversal(nums, left, mid - 1);
        root.right = traversal(nums, mid + 1, right);
        return root;
    }
}
```

2. 左闭右开

```Java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return traversal(nums, 0, nums.length);
    }
    public TreeNode traversal(int[] nums, int left, int right) {
        if (left >= right) {
            return null;
        }
        int mid = (left + right) / 2; 
        TreeNode root = new TreeNode(nums[mid]);
        root.left = traversal(nums, left, mid);
        root.right = traversal(nums, mid + 1, right);
        return root;
    }
}
```

注意，取中点最好写成`int mid = left + ((right - left) / 2);`避免数组越界。这种写法相当于是如果数组长度为偶数，中间位置有两个元素，取靠左边的。

## 538. 把二叉搜索树转换为累加树

看一下代码随想录。如果这个是个有序数组，情况就容易理解很多。现在映射到二叉搜索树上，体会“反中序遍历”的过程。

```Java
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        traversal(root);
        return(root);
    }
    public void traversal(TreeNode root) {
        if (root == null) {
            return;
        }
        // 反中序遍历的顺序
        traversal(root.right); // 右
        sum += root.val; // 累计求和
        root.val = sum; // 赋给当前节点
        traversal(root.left); // 左
    }
}
```



```java
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```
