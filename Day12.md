# Day 12

## 150. 逆波兰表达式求值

本题理解题意后并不难，主要是减法和除法的逻辑 + 字符串转换为数字的写法。

```java
class Solution {
    public int evalRPN(String[] tokens) {
        // Deque<Integer> stack = new LinkedList<>();
        Stack<Integer> stack = new Stack<Integer>();
        // Java中，String类型首字母大写！
        for (String s : tokens) {
            // String用""而非''，相等用equals()而非==
            if (s.equals("+")) {
                stack.push(stack.pop() + stack.pop());
            } else if (s.equals("-")) {
                stack.push(-stack.pop() + stack.pop()); // 先出来的是被减数
            } else if (s.equals("*")) {
                stack.push(stack.pop() * stack.pop());
            } else if (s.equals("/")) {
                int num1 = stack.pop();
                int num2 = stack.pop();
                stack.push(num2 / num1); // 先出来的是被除数，所以不能连用两个pop 
            } else {
                // stack.push(Integer.valueOf(s));
                stack.push(Integer.parseInt(s)); // 字符串转换数字
            }
        }
        return stack.pop();
    }
}
```
## 239. 滑动窗口最大值

可以参考视频：https://www.bilibili.com/video/BV1bM411X72E/?spm_id_from=333.788.recommend_more_video.1&vd_source=c13858a066dc9d4f42cfb786aee782a1

随着窗口的滑动，维护一个单调队列。第一个位置对应的元素，即当前窗口的最大值。

![239.滑动窗口最大值-2](/Users/anhejia/Downloads/239.滑动窗口最大值-2.gif)

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        // 长度为1就不用比了
      	if (nums == null || nums.length == 1) {
            return nums;
        }
        // 双端队列，单调递减。用于记录最大的若干个数的位置index
        Deque<Integer> deque = new ArrayDeque<>();
        int n = nums.length;
        // 窗口移动多少次，结果就返回几个数
        int[] res = new int[n - k + 1];
        for (int i = 0; i < n; i++) {
            // 1. 入
            // 从后往前，找到对应的位置，比它小的就不要了
            while (!deque.isEmpty() && nums[deque.getLast()] <= nums[i]) {
                deque.removeLast(); 
            }
            deque.addLast(i); // 入列
            // 2. 出
            // 一次挪一个，只有队列的最左的一个元素可能离开[i - k + 1, i]的范围
            if (deque.getFirst() < i - k + 1) { 
                deque.removeFirst();
            }
            // 3. 记录答案
            // 当i增长到符合第一个k范围的时候，第一个窗口形成
            if (i - k + 1 >= 0) {
                res[i - k + 1] = nums[deque.getFirst()];
            }
        }
        return res;
    }
}
```

### 347. 前k个最大元素

Comparator接口说明:
返回负数，形参中第一个参数排在前面；返回正数，形参中第二个参数排在前面

1. 对于队列：排在前面意味着往队头靠
2. 对于堆（使用PriorityQueue实现）：从队头到队尾按从小到大排就是最小堆（小顶堆），从队头到队尾按从大到小排就是最大堆（大顶堆）--> 队头元素相当于堆的根节点

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // 使用哈希表统计每个数字出现的频率
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        // 创建一个优先队列,按照频率从大到小排序
        PriorityQueue<int[]> pq = new PriorityQueue<>((pair1, pair2) -> pair2[1] - pair1[1]);
        // 遍历哈希表中的所有键值对,将其转换为数组并添加到优先队列中
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            pq.add(new int[]{entry.getKey(), entry.getValue()});
        }
        // 创建一个大小为 k 的数组,用于存储出现频率最高的 k 个数字
        int[] res = new int[k];
        // 从优先队列中取出前 k 个数字,存储到结果数组中
        for (int i = 0; i < k; i++) {
            res[i] = pq.poll()[0];
        }
        return res;
    }
}
```




## 
