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



## 
