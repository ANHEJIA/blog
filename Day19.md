# Day 19

## 530. 二叉搜索树中的最小绝对差

二叉搜索树的中序遍历，是一个有序数组。那求最小绝对差就很简单了。

不过，在二叉搜素树中序遍历的过程中，我们就可以直接计算了。需要用一个pre节点记录一下cur节点的前一个节点。

```java
class Solution {
    TreeNode pre; // 记录上一个遍历的结点
    int res = Integer.MAX_VALUE; // 记录最小绝对差
    
    public int getMinimumDifference(TreeNode root) {
        if (root == null)
            return 0;
        traversal(root);
        return res;
    }

    public void traversal(TreeNode node) {
        if (node == null) {
            return;
        }
        // 左
        traversal(node.left);
        // 中
        if (pre != null) {
            res = Math.min(res, node.val - pre.val);
        }
        pre = node;
        // 右
        traversal(node.right);
    }
}
```

## 501. 二叉搜索树中的众数

如果不是二叉搜索树，最直观的方法是把这个树都遍历了，用map统计频率，把频率排个序，最后取前面高频的元素的集合。

**既然是搜索树，它中序遍历就是有序的**。那么，可以先遍历比较一遍，再遍历一遍收集结果。

这边给出的方法，只需要遍历一遍：

```Java
class Solution {
    ArrayList<Integer> resList; 
    int maxCount; 
    int count; 
    TreeNode pre;

    public int[] findMode(TreeNode root) {
        // 初始化
        resList = new ArrayList<>(); 
        maxCount = 0; 
        count = 0;
        pre = null; 
        // 递归遍历树以找到众数
        traversal(root); 
        // int[]创建时要固定大小，所以先从可变的ArrayList<Integer>入手，再转换
        int[] res = new int[resList.size()]; 
        for (int i = 0; i < resList.size(); i++) {
            res[i] = resList.get(i); 
        }
        return res; 
    }

    public void traversal(TreeNode root) {
        if (root == null) {
            return;
        }
        traversal(root.left); // 左

        // 如果当前节点是第一个节点或节点值发生变化，重置计数器为1
        if (pre == null || root.val != pre.val) {
            count = 1; 
        // 如果当前节点值与上一个节点值相同，计数器加1
        } else {
            count++; 
        }
        // 如果当前计数大于最大出现次数，改旗易帜
        if (count > maxCount) {
            resList.clear(); 
            resList.add(root.val);
            maxCount = count; 
        // 如果当前计数等于最大出现次数，加我一个
        } else if (count == maxCount) {
            resList.add(root.val);
        }
        // 若count没有超过maxCount，继续即可
        pre = root; 

        traversal(root.right); // 右
    }
}
```

## 236. 二叉树的最近公共祖先

两种情况考虑清楚即可。即：

1. p,q一个在左，一个在右，那就返回root
2. p,q是父子关系，返回父节点

```Java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 如果树为空，或者根节点等于p或q，则返回根节点。因为根节点就是最近的公共祖先
        if (root == null || root == p || root == q)
            return root;
        // 后序遍历
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        // 如果左子树中没有找到公共祖先，即left为null，则返回右子树的查找结果
        if (left == null)
            return right;
        // 如果右子树中没有找到公共祖先，即right为null，则返回左子树的查找结果
        if (right == null)
            return left;
        // 如果左右子树查找结果均不为空，说明p和q分别位于root的两侧，root即为最近的公共祖先
        return root;
    }
}
```

