# Day7

## 454. 四数相加ll

本题核心：不需要对结果去重，所以简单统计次数就行。拆为两个双循环，一个负责加，一个负责减。

```Java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int res = 0;
        // 统计两个数组中的元素之和，同时统计出现的次数，放入map
        Map<Integer, Integer> map = new HashMap<>();
        for (int i : nums1) {
            for (int j : nums2) {
                map.put(i + j, map.getOrDefault(i + j, 0) + 1);
            }
        }
        // 统计剩余的两个元素的和，在map中找是否存在相加为0的情况，同时记录次数
        for (int i : nums3) {
            for (int j : nums4) {
                res += map.getOrDefault(0 - i - j, 0);
            }
        }
        return res;
    }
}
```

## 383. 赎金信

简单情况，用数组而非哈希表，省地方。

```Java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        // 长度不符，直接排除
        if (ransomNote.length() > magazine.length()) {
            return false;
        }
        int[] record = new int[26];
        // 有的
        for (char c : magazine.toCharArray()) {
            record[c - 'a']++;
        }
        // 要的
        for (char c : ransomNote.toCharArray()) {
            record[c - 'a']--;
        }
        // 注意这里的写法
        for (int i : record) {
            // 有无亏欠的
          	if (i < 0) {
                return false;
            }
        }
        return true;
    }
}
```

## 15. 三数之和

这题的难点，主要在于去重。**不能有重复的三元组，但三元组内的元素是可以重复的。**

哈希解法比较费时，而且处理去重比较麻烦。

采用双指针法，并且配合一定的优化手段。

```Java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        int n = nums.length;
        // 第一层循环
        for (int i = 0; i < n - 2; i++) {
            int cur = nums[i];
            // 出现重复数字，跳过
            if (i > 0 && cur == nums[i - 1]) {
                continue;
            }
            // 优化1（可去）:从cur开始，连着的数都太大了，跳出
            if (cur + nums[i + 1] + nums[i + 2] > 0) {
                break;
            }
            // 优化2（可去）:cur加上最大的两个数都不够，cur太小，跳过
            if (cur + nums[n - 1] + nums[n - 2] < 0) {
                continue;
            }
            // 双指针求解两数之和
            int j = i + 1;
            int k = n - 1;
            while (j < k) {
                int sum = cur + nums[j] + nums[k];
                if (sum > 0) {
                    k--;
                } else if(sum < 0) {
                    j++;
                } else {
                    res.add(List.of(cur, nums[j], nums[k]));
                    // j指针往后移动，直到不与前一个数字重复。起到跳过重复数字的效果
                    for (j++; j < k && nums[j] == nums[j - 1]; j++);
                    for (k--; k > j && nums[k] == nums[k + 1]; k--);
                }
            }
        }
        return res;
    }
}
```

## 18. 四数之和

基本上是三数之和再套一层循环。略。
