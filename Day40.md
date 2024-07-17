# Day 39

## 198. 打家劫舍

dp[i]：考虑下标i（包括i）以内的房屋，最多可以偷窃的金额为dp[i]。

决定dp[i]的因素就是第i房间偷还是不偷。

如果偷第i房间，那么dp[i] = dp[i - 2] + nums[i] ，即：第i-1房一定是不考虑的，找出 下标i-2（包括i-2）以内的房屋，最多可以偷窃的金额为dp[i-2] 加上第i房间偷到的钱。

如果不偷第i房间，那么dp[i] = dp[i - 1]，即考虑i-1房，（**注意这里是考虑，并不是一定要偷i-1房，这是很多同学容易混淆的点**）

```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(dp[0], nums[1]);
        for (int i = 2; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        return dp[nums.length - 1];
    }
}
```

## 213. 打家劫舍ll

因为首尾相连成环，所以不能都偷，考虑哪个不偷。主要考虑三种情况：

1. 首尾元素都跳过
2. 尾元素跳过
3. 首元素跳过

因为第一种情况包括在后两种之中，所以比较后边两种方案的最大值即可。

```Java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }
        int res1 = robRange(nums, 0, nums.length - 2);
        int res2 = robRange(nums, 1, nums.length - 1);
        return Math.max(res1, res2);
    }
    // 198.打家劫舍的逻辑
    public int robRange(int[] nums, int start, int end) {
        if (start == end) {
            return nums[start];
        }
        int[] dp = new int[nums.length];
        dp[start] = nums[start];
        dp[start + 1] = Math.max(nums[start], nums[start + 1]);
        for (int i = start + 2; i <= end; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        return dp[end];
    }
}
```

## 337. 打家劫舍lll

和前两题的区别，主要在于本题换成了树。

**本题一定是要后序遍历，因为通过递归函数的返回值来做下一步计算**。关键是要讨论当前节点抢还是不抢。如果抢了当前节点，两个孩子就不能动，如果没抢当前节点，就可以考虑抢左右孩子。

方法一：直接递归会超时，原因是重复计算。所以可以用hashmap记录。

方法二：状态标记递归。动态规划其实就是使用状态转移容器来记录状态的变化，这里可以使用一个长度为2的数组，记录当前节点偷与不偷所得到的的最大金钱。

不偷：Max(左孩子不偷，左孩子偷) + Max(右孩子不偷，右孩子偷)

偷：左孩子不偷+ 右孩子不偷 + 当前节点偷

```Java
class Solution {
    public int rob(TreeNode root) {
        int[] res = robAction(root);
        return Math.max(res[0], res[1]);
    }

    public int[] robAction(TreeNode root) {
        int res[] = new int[2];
        if (root == null) {
            return res;
        }
        int[] left = robAction(root.left);
        int[] right = robAction(root.right);
        res[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        res[1] = root.val + left[0] + right[0];
        return res;
    }
}
```
