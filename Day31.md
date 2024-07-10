# Day 31

## **56. 合并区间**

注意这里对数据结构的使用以及转换。

```Java
class Solution {
    public int[][] merge(int[][] intervals) {
        // 不确定最后的区间数量，先用链表存储
        List<int[]> res = new LinkedList<>();
        // 按照左边界排序
        Arrays.sort(intervals, (x, y) -> {return x[0] - y[0];});
        // initial start 是最小左边界
        int start = intervals[0][0];
        int rightmost = intervals[0][1];
        for (int i = 1; i < intervals.length; i++) {
            // 如果左边界大于最大右边界
            if (intervals[i][0] > rightmost) {
                // 加入区间 并且更新start
                res.add(new int[] { start, rightmost});
                start = intervals[i][0];
                rightmost = intervals[i][1];
            } else {
                // 更新最大右边界
                rightmost = Math.max(rightmost, intervals[i][1]);
            }
        }
        // 将合并后的区间，存入res中
        res.add(new int[] {start, rightmost});
        // 将链表res中的结果，转换为Array
        return res.toArray(new int[res.size()][]);
    }
}
```

## **738. 单调递增的数字**

本题只要想清楚个例，例如98，一旦出现strNum[i - 1] > strNum[i]的情况（非单调递增），首先想让strNum[i - 1]减一，strNum[i]赋值9，这样这个整数就是89。就可以很自然想到对应的贪心解法了。

想到了贪心，还要考虑遍历顺序，只有从后向前遍历才能重复利用上次比较的结果。

```Java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        String s = String.valueOf(n);
        char[] chars = s.toCharArray();
        int start = s.length();
        for (int i = s.length() - 2; i >= 0; i--) {
            if (chars[i] > chars[i + 1]) {
                chars[i]--;
                start = i+1;
            }
        }
        for (int i = start; i < s.length(); i++) {
            chars[i] = '9';
        }
        // 注意这边的转换方法
        return Integer.parseInt(String.valueOf(chars));
    }
}
```

## **968. 监控二叉树（可跳过）**
