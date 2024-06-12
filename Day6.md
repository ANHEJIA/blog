# Day 6

## 242. 有效的字母异位词

**数组其实就是一个简单哈希表**！而且这道题目中字符串只有小写字符，那么就可以定义一个长度为26的数组，来记录字符串s里字符出现的次数。

当然，直接用HashMap也是可以的。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] records = new int[26];
        // 一个录，一个销
      	for (int i = 0; i < s.length(); i++) {
            records[s.charAt(i) - 'a']++;
        }
        for (int i = 0; i < t.length(); i++) {
            records[t.charAt(i) - 'a']--;
        }
        // 看看有没有数量对不上的情况
        for (int record: records) {
            if (record != 0) {
                return false;
            }
        }
        return true;
    }
}
```

但是要注意，**使用数组来做哈希的题目，是因为题目都限制了数值的大小。**如果没有限制数值的大小，就无法使用数组来做哈希表了。

**而且如果哈希值比较少、特别分散、跨度非常大，使用数组就造成空间的极大浪费。**

## 349. 两个数组的交集

不能直接int[] res = new int[];，后边需要指定具体长度。所以还是值得借助另一个Set。

```Java
import java.util.HashSet;
import java.util.Set;
import java.util.ArrayList;

class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<>();
        Set<Integer> resultSet = new HashSet<>();
        
        // 将 nums1 中的元素加入 set
        for (int num : nums1) {
            set.add(num);
        }
        
        // 查找 nums2 中的交集元素
        for (int num : nums2) {
            if (set.contains(num)) {
                resultSet.add(num);
            }
        }
        
        // 将结果转换为数组
        int[] res = new int[resultSet.size()];
        int i = 0;
        for (int num : resultSet) {
            res[i++] = num;
        }
        
        return res;
    }
}
```

## 202. 快乐数

重点学习 得到下一个数 的逻辑！

```Java
class Solution {
    public boolean isHappy(int n) {
        // 用哈希表的原因就在于其后生成的数是否是之前重复的，达到直接去重的效果
        Set<Integer> set = new HashSet<>();
        // 要么出循环=1，要么是陷入了“循环”，从而跳出
        while (n != 1 && !set.contains(n)){
            set.add(n);
            n = getNextNum(n);
        }
        // 判断n跳出循序时，是否等于1的简略写法
        return n == 1;
    }
    // 得到下一个数的方法 
    public int getNextNum(int n) {
        int sum = 0;
        // 重点逻辑
        while (n > 0) {
            int d = n % 10;
            sum += d * d;
            n = n / 10;
        }
        return sum;
    }
}
```

## 1. 两数之和

经典例题，必须拿下。

```Java
class Solution {
    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.get(target - nums[i]) != null) {
                // 返回位置，第一个元素是a在nums中的下标,第二个元素是b在nums中的下标
                return new int[]{map.get(target - nums[i]), i};
            }
            map.put(nums[i], i);
        }
        return new int[]{};
    }
}
```

