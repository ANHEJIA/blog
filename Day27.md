# Day 27

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。一般来说，贪心没有套路，说白了就是常识性推导加上举反例。

刷题或者面试的时候，手动模拟一下感觉可以局部最优推出整体最优，而且想不到反例，那么就试一试贪心。

## 455. 分发饼干

思路1：优先考虑饼干，小饼干先喂饱小胃口

```Java
class Solution {
    // 孩子i，胃口值g[i]; 饼干j,饼干尺寸s[j]
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int cnt = 0;
        int cur = 0; // 现在考虑喂饱的小孩序号
        for (int j = 0; j < s.length && cur < g.length; j++) {
            // 可以喂饱，计数，换下一个小孩
            if (s[j] >= g[cur]) {
                cur++;
                cnt++;
            }
        }
        return cnt;
    }
}
```

思路2：优先考虑胃口，先喂饱大胃口

```java
class Solution {
    // 孩子i，胃口值g[i]; 饼干j,饼干尺寸s[j]
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int cnt = 0;
        int cur = s.length - 1; // 现在考虑使用的饼干序号
        for (int i = g.length - 1; i >= 0 && cur >= 0; i--) { // 注意这边应该都用>=
            // 可以喂饱，计数，换下一块饼干
            if (s[cur] >= g[i]) {
                cur--;
                cnt++;
            }
        }
        return cnt;
    }
}
```

## **376. 摆动序列**

看一下代码随想录，画图更容易理解。本题需要考虑特殊情况。

```Java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length <= 1) {
            return nums.length;
        }
        int curDiff = 0; // 当前差值
        int preDiff = 0; // 上一个差值
        int count = 1; // 只要有2个数，就默认会有1个峰值
        for (int i = 1; i < nums.length; i++) {
            // 得到当前差值
            curDiff = nums[i] - nums[i - 1];
            // 出现峰值
            if ((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)) {
                count++;
                preDiff = curDiff; // 放到括号内，只在摆动变化的时候更新prediff
            }
        }
        return count;
    }
}
```

## **53. 最大子序和**

遍历 nums，从头开始用 count 累积，如果 count 一旦加上 nums[i]变为负数，那么就应该从 nums[i+1]开始从 0 累积 count 了，因为已经变为负数的 count，只会拖累总和。

**这相当于是暴力解法中的不断调整最大子序和区间的起始位置**。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE;
        int sum = 0;
        for(int i = 0; i < nums.length; i++) {
            sum += nums[i];
            maxSum = Math.max(maxSum, sum);
            if (sum < 0) {
                sum = 0; // 相当于重置最大子序起始位置，因为遇到负数一定是拉低总和
            }
        }
        return maxSum;
    }
}
```

本题使用动态规划解决同样十分经典！
