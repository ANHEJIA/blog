# Day 30

## **452. 用最少数量的箭引爆气球**

直观的想其实很简单，就是一箭多雕，能拼就拼。精髓是更新最小右边界那里，相当于i先决定和i-1拼一支箭；至于i+1能不能拼这一支，得看它凑不凑的进这个区间里。如果不凑，那么新的一支箭省不了。

```Java
class Solution {
    public int findMinArrowShots(int[][] points) {
        // 根据气球直径的开始坐标从小到大排序
        // 使用Integer内置比较方法，不会溢出
        Arrays.sort(points, (a, b) -> Integer.compare(a[0], b[0]));
        // points 不为空，至少需要一支箭
        int count = 1;  
        for (int i = 1; i < points.length; i++) {
            // 气球i和气球i-1不挨着，注意这里不是>=
            if (points[i][0] > points[i - 1][1]) {  
                count++;
            // 气球i和气球i-1挨着
            } else {  
                // 更新重叠气球最小右边界
                // 相当于i先决定和i-1拼一支箭；至于i+1能不能拼这一支，得看它凑不凑
                points[i][1] = Math.min(points[i][1], points[i - 1][1]); 
            }
        }
        return count;
    }
}
```

## **435. 无重叠区间**

本题和452非常像，弓箭的数量就相当于是彼此不交叉的区间的数量。只要把弓箭那道题目代码里射爆气球的判断条件加个等号（认为`[0，1]`和`[1，2]`不是相邻区间），然后用总区间数减去弓箭数量，就是要移除的区间数量了。

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        int count = 1;
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] >= intervals[i - 1][1]) {
                count++;
            } else {
                intervals[i][1] = Math.min(intervals[i][1], intervals[i - 1][1]);
            }
        }
        return intervals.length - count;
    }
}
```

## **763. 划分字母区间**

在遍历的过程中相当于是要找每一个字母的边界，**如果找到之前遍历过的所有字母的最远边界，说明这个边界就是分割点了**。此时前面出现过所有字母，最远也就到这个边界了。

可以分为如下两步：

- 统计每一个字符最后出现的位置
- 从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点

```java
class Solution {
    public List<Integer> partitionLabels(String s) {
        List<Integer> res = new ArrayList<>();
        int[] edges = new int[26];
        char[] chars = s.toCharArray();
        // 更新每个字母最远出现的位置
        for (int i = 0; i < chars.length; i++) {
            edges[chars[i] - 'a'] = i;
        }
        int curEdge = 0; // 之前遍历过的所有字母的最远边界
        int last = -1; // 上一个区间的结束位置
        for (int i = 0; i < chars.length; i++) {
            // 更新最远边界
            curEdge = Math.max(curEdge, edges[chars[i] - 'a']);
            // 找到最远边界
            if (i == curEdge) {
                res.add(i - last);
                last = i;
            }
        }
        return res;
    }
}
```
