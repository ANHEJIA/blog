# Day 2

## **977. 有序数组的平方**

最直接的方法：

每个都平方，然后直接调用排序方法。

```Java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] res = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            res[i] = nums[i] * nums[i];
        }
        Arrays.sort(res);
        return res;
    }
}
```

但是，没有用到文中的重要条件，这个数组本身是有序的。

所以用双指针的方法，两边比较平方大小。

```Java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int[] res = new int[nums.length];
        for (int i = nums.length - 1; i >= 0; i--) {
            if (nums[left] * nums[left] >= nums[right] * nums[right]) {
                res[i] = nums[left] * nums[left];
                left++;
            } else {
                res[i] = nums[right] * nums[right];
                right--;
            }
        }
        return res;
    }
}
```

## **209. 长度最小的子数组**

利用滑动窗口的思想。

```Java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int minLen = Integer.MAX_VALUE;
        int winSum = 0;
        for (int right = 0; right < nums.length; right++) {
            winSum += nums[right];
            // 满足条件
            while (winSum >= target) {
                // 更新最小长度
                minLen = Math.min(minLen, right - left + 1);
                winSum -= nums[left];
                left++;
            }
        }
        // 如果不存在，此时minLen会是Integer.MAX_VALUE，题目要求此时返回0
        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```

## **59. 螺旋矩阵II**

本题其实并不考算法，就是模拟过程和边界条件。

注意，`matrix[i1][j1++] = ++num;` 与`matrix[i1][j1++] = num++;`不一样！前者是先对num++，再赋值，再对j1++。

```Java
class Solution {
    public int[][] generateMatrix(int n) {
        int num = 0;
        int[][] matrix = new int[n][n];
        // 左上角元素坐标初始化
        int i1 = 0;
        int j1 = 0;
        // 右上角元素坐标初始化
        int i2 = n - 1;
        int j2 = n - 1;
        // 每轮缩小圈的范围
        while (i1 <= i2 && j1 <= j2) {
            // 处理只剩一行的情况
            if (i1 == i2) {
                while (j1 <= j2)
                    matrix[i1][j1++] = ++num;
            }
            // 处理只剩一列的情况
            if (j1 == j2) {
                while (i1 <= i2)
                    matrix[i1++][j1] = ++num;
            }
            // 从左向右遍历
            for (int j = j1; j < j2; j++) {
                matrix[i1][j] = ++num;
            }
            // 从上向下遍历
            for (int i = i1; i < i2; i++) {
                matrix[i][j2] = ++num;
            }
            // 从右向左遍历
            for (int j = j2; j > j1; j--) {
                matrix[i2][j] = ++num;
            }
            // 从下向上遍历
            for (int i = i2; i > i1; i--) {
                matrix[i][j1] = ++num;
            }
            // 缩小圈的边界
            i1++;
            j1++;
            i2--;
            j2--;
        }
        return matrix;
    }
}
```

