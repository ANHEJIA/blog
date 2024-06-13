# Day 8

## 344. 反转字符串

主要思想：两头对换！

```Java
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;
        while (left < right) {
            // 对换
            char temp = s[right];
            s[right] = s[left];
            s[left] = temp;
            // 向中间汇合
            left++;
            right--;
        }
    }
}
```

补充：异或法也可以实现值的对换，而无需使用临时变量。

## 541. 反转字符串ll

基于344，关键是通过循环 + min函数，实现题目的逻辑。

```Java
class Solution {
    public String reverseStr(String s, int k) {
        int n = s.length();
        char[] arr = s.toCharArray();
        // 每段选2k个字符
        for (int i = 0; i < n; i += 2 * k) {
            // 选择k个数，或者剩下的数（不足k个）。用min函数统一起来是精髓！
            reverse(arr, i, Math.min(i + k - 1, n - 1));
        }
        // 若调用 arr.toString()，得到的是数组对象的地址表示
        return new String(arr);
    }
    // 同344题：反转 arr 数组从 left 到 right 的部分
    public void reverse(char[] arr, int left, int right) {
        while (left < right) {
            char temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            left++;
            right--;
        }
    }
}
```

## 替换数字

题目本意是一道C++的题。Java里String是不可变的，顺便温习一下StringBuilder写法。

```Java
import java.util.Scanner;

class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String s = in.nextLine();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            if (Character.isDigit(s.charAt(i))) {
                sb.append("number");
            }else sb.append(s.charAt(i));
        }
        System.out.println(sb);
    }
}
```

