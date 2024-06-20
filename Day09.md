# Day9

## 151. 反转字符串中的单词

Day 8题目的综合应用。

```Java
class Solution {
    public String reverseWords(String s) {
        // 除去开头和末尾的空白字符
        s = s.trim();
        // 正则匹配连续的空白字符作为分隔符分割
        String[] words = s.split("\\s+");
        // 使用StringBuilder来构建最终的翻转字符串
        StringBuilder sb = new StringBuilder();
        // 从尾部向前遍历单词数组，实现翻转的效果
        for (int i = words.length - 1; i >= 0; i--) {
            sb.append(words[i]); // 添加单词
            if (i > 0) { // 如果不是第一个单词，添加一个空格
                sb.append(" ");
            }
        }
        return sb.toString();
    }
}
```

## 左反转字符串

同剑指offer 58 题目二，左翻转字符串。

简单来说，就是先全翻转，然后分段各自翻转。局部“负负得正”，整体“移花接木”。

```Java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        String s = in.nextLine();

        int len = s.length();  //获取字符串长度
        char[] chars = s.toCharArray();
        reverseString(chars, 0, len - 1);  //反转整个字符串
        reverseString(chars, 0, n - 1);  //反转前一段字符串，此时的字符串首尾尾是0,n - 1
        reverseString(chars, n, len - 1);  //反转后一段字符串，此时的字符串首尾尾是n,len - 1
        
        System.out.println(chars);

    }

    public static void reverseString(char[] ch, int start, int end) {
        //异或法反转字符串，参照题目 344.反转字符串的解释
        while (start < end) {
            ch[start] ^= ch[end];
            ch[end] ^= ch[start];
            ch[start] ^= ch[end];
            start++;
            end--;
        }
    }
}
```

## KMP算法

二刷时补。
