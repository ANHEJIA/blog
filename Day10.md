# Day 10

## 232. 用栈实现队列

主要是定义顺序。这边令栈底为队首，栈顶为队尾。

```java
class MyQueue {
    private Stack<Integer> stack1;// 实现栈
    private Stack<Integer> stack2;// 辅助栈

    public MyQueue() {
        stack1 = new Stack();
        stack2 = new Stack();
    }

    // 元素队尾入队
    public void push(int x) {
        stack1.push(x);
    }

    // 从队列的开头移除并返回元素
    public int pop() {
        // 放到辅助队列中
        while (!stack1.isEmpty()) {
            stack2.push(stack1.pop());
        }
        // 第二个栈栈顶就是最后进来的元素，即队首
        int res = stack2.pop();
        // 再将第二个栈的元素放回第一个栈
        while (!stack2.isEmpty()) {
            stack1.push(stack2.pop());
        }
        return res;
    }

    // 返回队首元素
    public int peek() {
        while (!stack1.isEmpty()) {
            stack2.push(stack1.pop());
        }
        // 第二个栈栈顶就是最先进来的元素，即队首
        int res = stack2.peek();
        // 再将第二个栈的元素放回第一个栈
        while (!stack2.isEmpty()) {
            stack1.push(stack2.pop());
        }
        return res;
    }

    // 判断队列是否为空
    public boolean empty() {
        return stack1.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

## 225. 用队列实现栈

还是一样，定义栈底为队首，栈顶为队尾。

### 方法一：两个队列

此时，只能对queue1的头部进行操作。另外，注意是要使用add方法！

```java
class MyStack {
    Queue<Integer> queue1;
    Queue<Integer> queue2;

    // 初始化队列
    public MyStack() {
        queue1 = new LinkedList<Integer>();
        queue2 = new LinkedList<Integer>();
    }

    // 入栈
    public void push(int x) {
        queue1.add(x);
    }

    // 移除并返回栈顶元素
    public int pop() {
        if (queue1.isEmpty()) {
            return -1;
        }
        int size = queue1.size();
        for (int i = 0; i < size - 1; i++) {
            queue2.add(queue1.poll());
        }
        int res = queue1.poll();
        while (!queue2.isEmpty()) {
            queue1.add(queue2.poll());
        }
        return res;
    }

    // 返回栈顶元素，即找到队列的最后一个元素
    public int top() {
        if (queue1.isEmpty()) {
            return -1;
        }
        int res = 0;
        for (int num : queue1) {
            res = num;
        }
        return res;
    }

    // 判断栈是否为空
    public boolean empty() {
        return queue1.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

### 方法二：双端队列

此时，可以对queue的尾部进行操作了。直接调用api即可。注意新建deque的方法。

```java
class MyStack {
    Deque<Integer> q;
    public MyStack() {
        q = new ArrayDeque<Integer>();
    }
    
    public void push(int x) {
        q.add(x);
    }
    
    public int pop() {
        return q.removeLast();
    }
    
    public int top() {
        return q.getLast();
    }
    
    public boolean empty() {
        return q.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

## 20. 有效的括号

碰到左括号，就把右括号存进栈里，等着碰到右括号的时候核销。

```java
class Solution {
    public boolean isValid(String s) {
        // 如果字符串长度为奇数，直接返回false
        if (s.length() % 2 != 0) {
            return false;
        }
        // 初始化栈，用于存放括号
        Stack<Character> stack = new Stack<>();
        // 遍历字符串中的每个字符
        for (char c : s.toCharArray()) {
            // 如果是左括号，压入相对应的右括号
            if (c == '(') {
                stack.push(')');
            } else if (c == '[') {
                stack.push(']');
            } else if (c == '{') {
                stack.push('}');
            }
            // 1. 右括号先于左括号出现，所以此时栈为空，返回false
            // 2. 右括号，栈顶元素不匹配，返回false
            else if (stack.isEmpty() || stack.pop() != c) {
                return false;
            }
        }
        
        // 如果栈为空，说明所有括号都正确匹配，返回true。否则，返回false
        return stack.isEmpty();
    }
}
```

## 1047. 删除字符串中的所有相邻重复项

用sb模拟一个栈。

```java
class Solution {
    public String removeDuplicates(String s) {
        StringBuilder sb = new StringBuilder();
        // 对于s中的每一个字符c
        for (char c : s.toCharArray()) {
            // 没有字母，直接添加
            if (sb.length() == 0) {
                sb.append(c);
            }
            // 若c与刚刚添加的字母相同，消去
            else if (sb.charAt(sb.length() - 1) == c) {
                sb.deleteCharAt(sb.length() - 1);
            // 不满足相消条件，正常添加
            } else {
                sb.append(c);
            }
        }
        return sb.toString();
    }
}
```



# 
