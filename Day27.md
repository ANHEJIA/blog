# Day 27

## 121. 买卖股票的最佳时机

经典题目，和53题很类似。

```Java
class Solution {
    public int maxProfit(int[] prices) {
        int cost = Integer.MAX_VALUE;
        int profit = 0;
        for (int price : prices) {
            cost = Math.min(cost, price); // 如果当前价格小于之前的最小价格，更新最小价格
            profit = Math.max(profit, price - cost); // 如果当前价格与最小价格之差大于之前的最大利润，更新最大利润
        }
        return profit;
    }
}
```

## **122. 买卖股票的最佳时机II**

**本题中理解利润拆分是关键点！** 不要整块的去看，而是把整体利润拆为每天的利润。

一旦想到这里了，很自然就会想到贪心了，即：只收集每天的正利润，最后稳稳的就是最大利润了。

```Java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        for (int i = 1; i < prices.length; i++) { // 需要从第二天开始算
            profit += Math.max(prices[i] - prices[i - 1], 0); // 考虑可以产生正利润的天数
        }
        return profit;
    }
}
```

## **55. 跳跃游戏** 

依次遍历数组中的每一个位置，并实时维护最远可以到达的位置。

```Java
public class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length; // 数组的长度
        int rightmost = 0; // 目前能跳到的最远距离
        for (int i = 0; i < n; i++) {
            // 如果当前位置可以到达（即当前位置i不大于目前能跳到的最远距离）
            if (i <= rightmost) {
                // 更新能跳到的最远距离
                rightmost = Math.max(rightmost, i + nums[i]);
                // 如果最远距离已经超过或者达到数组的最后一个位置，则可以到达，返回true
                if (rightmost >= n - 1) {
                    return true;
                }
            }
        }
        // 如果循环结束后，没有返回true，则说明不能跳到最后一个位置，返回false
        return false;
    }
}
```

## **45. 跳跃游戏II**

**需要统计两个覆盖范围，当前这一步的最大覆盖和下一步最大覆盖**。

如果移动下标达到了当前这一步的最大覆盖最远距离了，还没有到终点的话，那么就必须再走一步来增加覆盖范围，直到覆盖范围覆盖了终点。

```java
class Solution {
    public int jump(int[] nums) {
        int cnt = 0;
        // 当前覆盖的最远距离下标
        int end = 0;
        // 下一步覆盖的最远距离下标
        int nextend = 0;
        for (int i = 0; i <= end && end < nums.length - 1; i++) {
            nextend = Math.max(nextend, i + nums[i]);
            // 可达位置的改变次数就是跳跃次数
            if (i == end) {
                end = nextend;
                cnt++;
            }
        }
        return cnt;
    }
}
```

## **1005. K次取反后最大化的数组和**

先对数组进行排序，将尽可能多的负数取反，如果此时k还有剩余，排序，对最小的那个正数(因为k有剩余负数肯定全部取反了) 将k对2取模，对最小的正数也就是nums[0]，取k次反即可。

```Java
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        // 对数组进行升序排序
        Arrays.sort(nums);
        int sum = 0;
        // 从数组头开始，尽可能多地取反负数
        for (int i = 0; i < nums.length && k > 0; i++) {
            if (nums[i] < 0) {
                nums[i] = -nums[i];
                k--;
            } else {
                break;
            } 
        }
        // 再次对数组进行升序排序
        Arrays.sort(nums);
        // 如果k == 0，不能动
        // 如果剩余的k是偶数，对一个数反复取反，保持全是正数的状态
        // 如果剩余的k是奇数，则将全正数组中绝对值最小的元素取反
        nums[0] = k % 2 == 0 ? nums[0] : -nums[0];
        // 计算整个数组的元素之和并返回。
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }
        return sum;
    }
}
```
