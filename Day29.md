# Day 28

## **134. 加油站**

重点掌握第二种贪心方法。用局部最优可以推出全局最优，进而求得起始位置。

首先如果总油量减去总消耗大于等于零那么一定可以跑完一圈。

每个加油站的剩余量rest[i]为gas[i] - cost[i]。i从0开始累加rest[i]，和记为curSum，一旦curSum小于零，说明[0, i]区间都不能作为起始位置，因为这个区间选择任何一个位置作为起点，到i这里都会断油。那么起始位置从i+1算起，再从0计算curSum。

```Java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int curSum = 0;
        int totalSum = 0;
        int index = 0;
        // 验证跑完一圈的可行性
        for (int i = 0; i < gas.length; i++) {
            totalSum += gas[i] - cost[i];
        }
        if (totalSum < 0) {
            return -1;
        }
        for (int i = 0; i < gas.length; i++) {
            curSum += gas[i] - cost[i];
            // 是一个环，所以+1取余数，得到新的起始位置
            if (curSum < 0) {
                index = (i + 1) % gas.length; 
                curSum = 0;
            }
        }
        return index;
    }
}
```

## **135. 分发糖果**

如果在考虑局部的时候想两边兼顾，就会顾此失彼。

那么本题我采用了两次贪心的策略：

- 一次是从左到右遍历，只比较右边孩子评分比左边大的情况。
- 一次是从右到左遍历，只比较左边孩子评分比右边大的情况。

这样从局部最优推出了全局最优，即：相邻的孩子中，评分高的孩子获得更多的糖果。

```Java
class Solution {
    public int candy(int[] ratings) {
        int len = ratings.length;
        int[] nums = new int[len];
        nums[0] = 1;
        // 起点下标1，从左往右
        for (int i = 1; i < len; i++) {
            // 只要右边比左边大，右边的糖果=左边+1
            nums[i] = (ratings[i] > ratings[i - 1]) ? nums[i - 1] + 1 : 1;
        }
        // 起点下标ratings.length - 2，从右往左
        for (int i = len - 2; i >= 0; i--) {
            // 只要左边比右边大，此时左边的糖果应该取本身的糖果数（符合比它左边大）和 右边糖果数+1二者的最大值，这样才符合它比它左边的大，也比它右边大
            if (ratings[i] > ratings[i + 1]) {
                nums[i] = Math.max(nums[i], nums[i + 1] + 1);
            }
        }
        // 记录结果数目
        int res = 0;
        for (int num : nums) {
            res += num;
        }
        return res;
    }
}
```

## **860.柠檬水找零**

只需要维护三种金额的数量，5，10和20。

有如下三种情况：

- 情况一：账单是5，直接收下。
- 情况二：账单是10，消耗一个5，增加一个10
- 情况三：账单是20，优先消耗一个10和一个5，如果不够，再消耗三个5（贪心策略）
    此时，其实收到的20，对以后的找零毫无贡献。所以不用维护20的数量了。

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0;
        int ten = 0;
        for (int i = 0; i < bills.length; i++) {
            if (bills[i] == 5) {
                five++;
            } else if (bills[i] == 10) {
                five--;
                ten++;
            } else if (bills[i] == 20) {
                if (ten > 0) {
                    ten--;
                    five--;
                } else {
                    five -= 3;
                }
            }
            // 本轮“预支”了钞票，无法找零
            if (five < 0 || ten < 0) {
                return false;
            }
        }
        return true;
    }
}
```

## **406.根据身高重建队列**

本题有两个维度，h和k。和分发糖果类似，不要两头兼顾，处理好一边再处理另一边。 

算法实现不难，看下代码随想录。

**局部最优：优先按身高高的people的k来插入。插入操作过后的people满足队列属性**

**全局最优：最后都做完插入操作，整个队列满足题目队列属性**

```Java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        // 按照身高从大到小排序（如果身高相同，k值小的站前面）
        Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];   // 如果身高相同，按k值升序排列
            return b[0] - a[0];   // 如果身高不同，按身高降序排列
        });
        LinkedList<int[]> queue = new LinkedList<>();
        for (int[] p : people) {
            // LinkedList.add(index, value)，会将value插入到指定index位置
            queue.add(p[1], p);   
        }
        // 注意这边的转换
        return queue.toArray(new int[people.length][]);
    }
}
```
