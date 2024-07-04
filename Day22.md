# Day 22

回溯法中递归函数参数很难一次性确定下来，一般先写逻辑，需要啥参数了，填什么参数。

## 77. 组合

n相当于树的宽度，k相当于树的深度。

```Java
class Solution {
    // 需要多次修改，所以用链表效率更高
    LinkedList<Integer> path = new LinkedList<>();
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> combine(int n, int k) {
        backtracking(n, k, 1);
        return res;
    }
    public void backtracking(int n, int k, int start) {
        // 找到了k个，转换成ArrayList添加到res
        if (path.size() == k) {
            res.add(new ArrayList<>(path)); 
            return;
        } 
        for (int i = start; i <= n; i++) { // 此处还可以减枝
            path.add(i);
            backtracking(n, k, i + 1);
            path.removeLast();
        }
    }
}
```

## 216. 组合总和lll

类似上题，注意区别。

```Java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
        backtracking(n, 0, k, 1);
        return res;
    }
    public void backtracking(int target, int sum, int k, int start) {
        // 剪枝
        if (sum > target) { 
            return;
        }
        if (sum == target && path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        } 
        for (int i = start; i <= 9; i++) {
            sum += i;
            path.add(i);
            backtracking(target, sum, k, i + 1);
            sum -= i;
            path.removeLast();
        }
    }
}
```

当然，其实把n,k分别提出来也是可以的。

```Java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    int target;
    int num;
    public List<List<Integer>> combinationSum3(int k, int n) {
        target = n;
        num = k;
        backtracking(0, 1);
        return res;
    }
    public void backtracking(int sum, int start) {
        if (sum > target) {
            return;
        }
        if (sum == target && path.size() == num) {
            res.add(new ArrayList<>(path));
            return;
        } 
        for (int i = start; i <= 9; i++) {
            sum += i;
            path.add(i);
            backtracking(sum, i + 1);
            sum -= i;
            path.removeLast();
        }
    }
}
```



## 17. 电话号码的字母组合

用String[]储存结果，更省地方。

```Java
class Solution {
    List<String> res = new ArrayList<>();
    // 初始对应所有的数字，为了直接对应2-9，新增了两个无效的字符串""
    String[] numString = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return res;
        }
        backtracking(digits, 0);
        return res;
    }
    // sb存储中间结果
    StringBuilder sb = new StringBuilder();
    public void backtracking(String digits, int num) {
        if (num == digits.length()) {
            res.add(sb.toString());
            return;
        }
        // str 表示当前num对应的字符串
        String str = numString[digits.charAt(num) - '0'];
        for (int i = 0; i < str.length(); i++) {
            sb.append(str.charAt(i));
            backtracking(digits, num + 1);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```
