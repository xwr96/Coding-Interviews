对于 TwoSum 问题，一个难点就是给的数组无序。对于一个无序的数组，我们似乎什么技巧也没有，只能暴力穷举所有可能。

一般情况下，我们会首先把**数组排序再考虑双指针技巧**。TwoSum 启发我们，**HashMap 或者 HashSet** 也可以帮助我们处理无序数组相关的简单问题。

另外，设计的核心在于权衡，利用不同的数据结构，可以得到一些针对性的加强。

最后，如果 TwoSum I 中给的**数组是有序**的，应该如何编写算法呢？答案很简单，前文「双指针技巧汇总」写过：

```Java
int[] twoSum(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) {
            return new int[]{left, right};
        } else if (sum < target) {
            left++; // 让 sum 大一点
        } else if (sum > target) {
            right--; // 让 sum 小一点
        }
    }
    // 不存在这样两个数
    return new int[]{-1, -1};
}
```
## nSum问题的解法
nSum问题可以转化为两数之和、三数之……。

```Java
class Solution {
    List<List<Integer>> ans;
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        ans = new LinkedList<List<Integer>> ();
        // i的终止条件是还剩不到三个数
        for (int i = 0; i < nums.length - 3; ++i) {
            // 去重
            if ((i >= 1) && nums[i] == nums[i - 1]) continue;
            // 把当前选中的数用来满足target了 所以传入下层的target要减去该数
            threeSum(nums, i + 1, target - nums[i] );
        }

        return ans;
    }

    private void threeSum(int[] nums, int start, int target) {
        // i的终止条件是还剩不到两个数
        for (int i = start; i < nums.length - 2; i++) {
            // 去重
            if ((i > start) && nums[i] == nums[i - 1] ) continue;
            // start是起始点 则start - 1就是之前选择的第一个数下标
            // i是现在选的数 即第二个数的下标
            twoSum(nums, i + 1, target - nums[i], i, start - 1);
        }
    }

    private void twoSum(int[] nums, int start, int target, int cIndex, int dIndex) {
        int p1 = start;
        int p2 = nums.length - 1;
        while (p1 < p2) {
            int sum = nums[p1] + nums[p2];
            if (sum == target) {
                // cIndex 我们选的第一个数的下标 dIndex 同理既是选择的第二个数的下标
                // 加入答案集
                ans.add(Arrays.asList(nums[p1], nums[p2], nums[cIndex], nums[dIndex]));
                p1++;
                p2--;
                while (p1 < p2 && nums[p1] == nums[p1 - 1]) ++p1;//去重
                while (p1 < p2 && nums[p2] == nums[p2 + 1]) --p2;
            } else if (sum < target) {
                p1++;
            } else {
                p2--;
            }
        }
    }
}
```
