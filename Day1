# Day 1

## 704. 二分查找

本题的关键，是要确定区间开闭情况，这在整个题目中是一个不变量。

1.左闭右闭 [left, right]

```Java
class Solution {
    public static int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        // 两个指针相遇前，二分查找
        while (left <= right) { // 小于等于
            int mid = (left + right) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```

2.左闭右开 [left, right)

```Java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length; // right不含！所以这里不用-1！
        while (left < right) { // 小于
            int mid = (left + right) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > target) {
                right = mid; // target 在左区间，在[left, mid)中
            } else if (nums[mid] < target) { 
                left = mid + 1; // target 在右区间，在[mid + 1, right)中
            }
        }
        return -1;
    }
}
```

## 27. 移除元素

数组的元素在内存地址中是连续的，不能单独删除数组中的某个元素，只能覆盖。

```Java
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        for (int right = 0; right < nums.length; right++) {
            if (nums[right] != val) {
                // 要求是“原地修改”，相当于把不等于val的数“挪过来”
                nums[left] = nums[right];
                left++;
            }
        }
        // 返回新数组的长度
        return left;
    }
}
```


