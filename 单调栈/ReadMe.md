## 单调栈的使用规则

当我们遍历到一个位置 i 需要寻找数组中左边或者右边的所有数字和 nums[i] 的大小关系的题目，可以考虑一下单调栈。

所谓「单调栈」就是栈中的元素都是依次递增或者递减的，从而方便我们能维护好数组的一个区间内的「最大值」「次大值」等等。


## 查分数组

单点更新，范围查询，就用线段树。范围更新，单独查询，就用差分数组。
```Java
int[] diff = new int[nums.length];
// 构造差分数组
diff[0] = nums[0];
for (int i = 1; i < nums.length; i++) {
    diff[i] = nums[i] - nums[i - 1];
}

//还原数组
int[] res = new int[diff.length];
// 根据差分数组构造结果数组
res[0] = diff[0];
for (int i = 1; i < diff.length; i++) {
    res[i] = res[i - 1] + diff[i];
}
```
现在我们把差分数组抽象成一个类，包含increment方法(在区间nums[i,j]增加数字)和result方法：
```Java
class Difference {
    // 差分数组
    private int[] diff;

    public Difference(int[] nums) {
        assert nums.length > 0;
        diff = new int[nums.length];
        // 构造差分数组
        diff[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            diff[i] = nums[i] - nums[i - 1];
        }
    }

    /* 给闭区间 [i,j] 增加 val（可以是负数）*/
    public void increment(int i, int j, int val) {
        diff[i] += val;
        if (j + 1 < diff.length) {
            diff[j + 1] -= val;
        }
    }

    public int[] result() {
        int[] res = new int[diff.length];
        // 根据差分数组构造结果数组
        res[0] = diff[0];
        for (int i = 1; i < diff.length; i++) {
            res[i] = res[i - 1] + diff[i];
        }
        return res;
    }
}
```
## 相关题目
[1109. 航班预订统计](https://leetcode-cn.com/problems/corporate-flight-bookings/)
