# Day 13

### 二叉树的递归遍历（DFS）

前序，中序，后序，主要指的是根节点的位置。

前序：中左右

中序：左中右

后序：左右中

这三种都是DFS的算法。

### 二叉树的迭代遍历

本质是用栈，而非递归模拟了遍历的过程。

这个比较复杂，不是二叉树的重点。二叉树重点还是理解递归的过程。

### 二叉树的层序遍历

方法一：递归（DFS）

本质上，这种递归不算是层序遍历，还是深度遍历。只是在遍历的时候将节点保存到对应层的列表中了。

```Java
class Solution {
    List<List<Integer>> lists = new ArrayList<>();
    public List<List<Integer>> levelOrder(TreeNode root) {
        dfs(root, 0);
        return lists;
    }
    public void dfs(TreeNode node, int level) {
        // 节点真的存在，更新层数
        if (node == null) {
            return;
        }
        level++;
        // list的数量要和层数相同
        if (lists.size() < level) {
            // 否则，初始化一个新层的子list，存放新层的结果
            lists.add(new ArrayList<Integer>());
        }
        // lists的index从0开始，所以level层存储在lists的level - 1位置
        lists.get(level - 1).add(node.val);
        // 递归
        dfs(node.left, level);
        dfs(node.right, level);
    }
}
```

方法二：借助队列迭代（BFS）

对于队列Queue：

- 如果你希望在队列已满时避免异常，使用 `offer`。
- 如果你希望在队列已满时立即知道并处理异常，使用 `add`。

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // lists存储最终结果
        List<List<Integer>> lists = new ArrayList<>();
        if (root == null) {
            return lists;
        }

        // 建一个队列，用来存放每一层的节点
        Queue<TreeNode> queue = new ArrayDeque<>();
        // 先初始化，把第一层仅有的节点root
        queue.offer(root);

        // 后边没节点了进队了，说明到头了
        while (!queue.isEmpty()) {
            // 每个List保存每一层的结点
            List<Integer> list = new ArrayList<>();
            // 记录这一层的节点数，可以和后边进来的节点区分
            // queue.size()不能写在判断的括号内，因为会变化
            int size = queue.size();  
            // 这一层的节点，需要完成以下3步：
            while (size > 0) {
                // 1. 出队
                TreeNode node = queue.poll();
                size--;
                // 2. 结点值添加到本层list
                list.add(node.val);
                // 3. 若有左右子节点，依次放到队列后
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            lists.add(list);
        }
        return lists;
    }
}
```



# 
