# Day 25

## **491.递增子序列**

本题不能排序！因为是从原序列中，选取子序列，排序后就改变了原序列的结构。

本题要理清，引入hashset是为了什么？

```Java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        backtracking(nums, 0);
        return res;
    }
    public void backtracking(int[] nums, int start) {
        if (path.size() >= 2) { // 用size而非length
            res.add(new ArrayList(path));
            // 注意这里不要加return，要取树上的节点
        }
        // 使用set对本层元素进行去重
        HashSet<Integer> set = new HashSet<>();
        for (int i = start; i < nums.length; i++) {
            if (!path.isEmpty() && path.getLast() > nums[i]) {
                continue;
            }
            if (set.contains(nums[i])) {
                continue;
            }
            // 记录这个元素在本层用过了，本层后面不能再用了
            set.add(nums[i]); // 用add而非put
            path.add(nums[i]);
            backtracking(nums, i + 1);
            path.removeLast();
        }
    }
}
```

## **46.全排列** 

**首先排列是有序的，也就是说 [1,2] 和 [2,1] 是两个集合，这和之前分析的子集以及组合所不同的地方**。

可以看出元素1在[1,2]中已经使用过了，但是在[2,1]中还要在使用一次1，所以处理排列问题就不用使用startIndex了。

在`-10 <= nums[i] <= 10`的前提下，排列问题需要一个等长的used数组，标记已经选择的元素

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    boolean[] used; // boolean小写
    public List<List<Integer>> permute(int[] nums) {
        used = new boolean[nums.length]; // 传入nums长度，记录是否使用过
        backtracking(nums);
        return res;
    }
    public void backtracking(int[] nums) {
        if (nums.length == path.size()) {
            res.add(new ArrayList(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) {
                continue;
            }
            used[i] = true;
            path.add(nums[i]);
            backtracking(nums);
            path.removeLast();
            used[i] = false;
        }
    }
}
```

## **47.全排列 II**

这道题目和上边的区别在与**给定一个可包含重复数字的序列**，要返回**所有不重复的全排列**。

这里又涉及到去重了，首先需要排序，然后数层上不能重复取

```Java
class Solution {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    boolean[] used;
    public List<List<Integer>> permuteUnique(int[] nums) {
        used = new boolean[nums.length];
        Arrays.fill(used, false); // 初始化
        Arrays.sort(nums);
        backTrack(nums);
        return result;
    }

    private void backTrack(int[] nums) {
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            // i > 0 && nums[i] == nums[i - 1]保证了两数重复
            // used[i - 1] == true，说明两数在同⼀树⽀重复
            // used[i - 1] == false，说明两数在同⼀树层重复
            // 如果同⼀树层nums[i - 1]使⽤过则直接跳过
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            // 如果同⼀树⽀nums[i]没使⽤过开始处理
            if (used[i] == false) {
                used[i] = true; // 标记同⼀树⽀nums[i]使⽤过，防止同一树枝重复使用
                path.add(nums[i]);
                backTrack(nums);
                path.removeLast(); // 回溯，说明同⼀树层nums[i]使⽤过，防止下一树层重复
                used[i] = false; // 回溯
            }
        }
    }
}
```
