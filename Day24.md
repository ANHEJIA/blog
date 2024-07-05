# Day 24

### **93.复原IP地址** 

和131一样，是切割问题。**切割问题就可以使用回溯搜索法把所有可能性搜出来**

本题我们还需要一个变量dotCount，记录添加逗点的数量。本题明确要求只会分成4段，所以不能用切割线切到最后作为终止条件，而是分割的段数作为终止条件。dotCount为3说明字符串分成了4段了。

```Java
class Solution {
    List<String> result = new ArrayList<>();
    public List<String> restoreIpAddresses(String s) {   
        // 使用stringBuilder，故向字符串插入字符时无需复制整个字符串
        StringBuilder sb = new StringBuilder(s);
        backTracking(sb, 0, 0);
        return result;
    }
    // startIndex: 搜索的起始位置， dotCount:添加逗点的数量
    private void backTracking(StringBuilder s, int startIndex, int dotCount) {
        // 逗点数量为3时，分隔结束
        if (dotCount == 3) {
            // 判断第四段⼦字符串是否合法，如果合法就放进result中
            if (isValid(s, startIndex, s.length() - 1)) {
                result.add(s.toString());
            }
            return;
        }
        for (int i = startIndex; i < s.length(); i++) {
            if (isValid(s, startIndex, i)) {
                // 前边有效，加点
                s.insert(i + 1, '.');
                // 下⼀个⼦串的起始位置为i + 2，递归
                backTracking(s, i + 2, dotCount + 1);
                // 回溯，删掉点
                s.deleteCharAt(i + 1);
            } else {
                // 包含无效前缀，直接退出循环，避免不必要的检查
                break;
            }
        }
    }
    // 判断字符串s在左闭右闭区间[start, end]所组成的数字是否合法
    private boolean isValid(StringBuilder s, int start, int end) {
        if (start > end)
            return false;
        if (s.charAt(start) == '0' && start != end)
            return false;
        int num = 0;
        for (int i = start; i <= end; i++) {
            num = num * 10 + s.charAt(i) - '0';
            if (num > 255)
                return false;
        }
        return true;
    }
}
```

### **78.子集**

组合问题和分割问题都是收集树的叶子节点，而子集问题是找树的所有节点。**求取子集问题，不需要任何剪枝！因为子集就是要遍历整棵树**。

既然是无序，取过的元素不会重复取，写回溯算法的时候，for就要从startIndex开始，而不是从0开始。

什么时候for可以从0开始呢？求排列问题的时候，就要从0开始，因为集合是有序的，{1, 2} 和{2, 1}是两个集合。

```Java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> set = new LinkedList<>();
    public List<List<Integer>> subsets(int[] nums) {
        backtracking(nums, 0);
        return res;
    }
    public void backtracking(int[] nums, int start) {
        // 遍历这个树的时候，把所有节点都记录下来，就是要求的子集集合
        res.add(new ArrayList(set));
        // 终止条件可不加，因为此时本身就会返回
        if (start >= nums.length) {
            return;
        }
        for (int i = start; i < nums.length; i++) {
            set.add(nums[i]);
            backtracking(nums, i + 1);
            set.removeLast();
        }
    }
}
```

### **90.子集II**

同一树层上重复取就要过滤掉，同一树枝上就可以重复取，因为同一树枝上元素的集合才是唯一子集！

```Java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> set = new LinkedList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        // 排序之后，才能识别“相等”
        Arrays.sort(nums);
        backtracking(nums, 0);
        return res;
    }
    public void backtracking(int[] nums, int start) {
        // 遍历这个树的时候，把所有节点都记录下来，就是要求的子集集合
        res.add(new ArrayList(set));
        // 终止条件可不加，因为此时本身就会返回
        if (start >= nums.length) {
            return;
        }
        for (int i = start; i < nums.length; i++) {
            // 跳过当前树层使用过的、相同的元素
            if (i > start && nums[i] == nums[i - 1]) {
                continue;
            }
            set.add(nums[i]);
            backtracking(nums, i + 1);
            set.removeLast();
        }
    }
}
```
