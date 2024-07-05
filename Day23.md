# Day 23

### **39. 组合总和** 

本题中，集合里元素可以用无数次，那么和组合问题的差别，其实仅在于 start上的控制，不用再+1了

```Java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        // 排序，能够有效地进行剪枝操作
        Arrays.sort(candidates);
        backtracking(candidates, target, 0, 0);
        return res;
    }
    public void backtracking(int[] candidates, int target, int sum, int start) {
        if (sum == target) {
            res.add(new ArrayList(path));
            return;
        }
        for (int i = start; i < candidates.length; i++) {
            // 剪枝
            if (sum + candidates[i] > target) {
                break;
            }
            sum += candidates[i];
            path.add(candidates[i]);
            backtracking(candidates, target, sum, i);
            sum -= candidates[i];
            path.removeLast();
        }
    }
}
```

### **40.组合总和II** 

本题的核心考察点：去重逻辑。

元素在同一个组合内是可以重复的，怎么重复都没事，但两个组合不能相同。//

**所以我们要去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重**。

```Java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtracking(candidates, target, 0, 0);
        return res;
    }
    public void backtracking(int[] candidates, int target, int sum, int start) {
        if (target == sum) {
            res.add(new ArrayList(path));
            return;
        }
        for (int i = start; i < candidates.length; i++) {
            // 剪枝
            if (sum > target) {
                break;
            }
            // 去重的是同一树层上的“使用过”，并且要防止越界
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue;
            }
            sum += candidates[i];
            path.add(candidates[i]);
            // i + 1表示组内元素仅选取一次
            backtracking(candidates, target, sum, i + 1);
            sum -= candidates[i];
            path.removeLast();
        }
    }
}
```

### **131.分割回文串**

模拟切割线，其实就是index是上一层已经确定了的分割线，i是这一层试图寻找的新分割线。

```java
class Solution {
    List<List<String>> lists = new ArrayList<>();
    LinkedList<String> list = new LinkedList<>();
    public List<List<String>> partition(String s) {
        backtracking(s, 0);
        return lists;
    }
    public void backtracking(String s, int start) {
        if (start >= s.length()) {
            lists.add(new ArrayList(list));
            return;
        }
        for (int i = start; i < s.length(); i++) {
            if (isPalindrome(s, start, i)) {
                list.add(s.substring(start, i + 1)); // substring是左闭右开的
            } else {
                continue; // 起码要到目前为止，是回文的，否则结束本轮
            }
            backtracking(s, i + 1); // 起始位置后移，保证不重复
            list.removeLast(); 
        }
    }
    // 左闭右闭，判断是否为回文
    public boolean isPalindrome(String s, int start, int end) {
        // 不能写成int i = start, int j = end;
        for (int i = start, j = end; i < j; i++, j--) {
            if (s.charAt(i) != s.charAt(j)) {
                return false;
            }
        }
        return true;
    }
}
```
