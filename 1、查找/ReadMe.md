## 二分查找基本框架（二分查找要求数组是有序的）

```Java
int binarySearch(int[] nums, int target) {
    int left = 0, right = ...;

    while(...) {
        int mid = left + (right - left) / 2;  //防止溢出
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...
        } else if (nums[mid] > target) {
            right = ...
        }
    }
    return ...;
}
```
## 寻找左侧边界的二分搜索(对于搜索左右侧边界的二分查找，左闭右开这种写法比较普遍)

```Java
public int left_bound(int[] nums,int target){
    if(nums.length==0) return -1;
    int left=0,right=nums.length; // 注意
    while(left<right){     // 注意
        int mid=(left+right)/2;
        if(nums[mid])==target){
            right=mid;
        }else if(nums[mid]<target){
            left=mid+1;
        }else if(nums[mid]>target){
            right=mid;  // 注意
        }
    }
    if (left == nums.length) return -1;
    // 类似之前算法的处理方式
    return nums[left] == target ? left : -1;
}
```
另一种左闭右闭的写法：
```Java
int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    // 搜索区间为 [left, right]
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            // 搜索区间变为 [mid+1, right]
            left = mid + 1;
        } else if (nums[mid] > target) {
            // 搜索区间变为 [left, mid-1]
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 收缩右侧边界
            right = mid - 1;
        }
    }
    // 检查出界情况
    if (left >= nums.length || nums[left] != target)
        return -1;
    return left;
}
```

## 寻找右侧边界的二分搜索
```Java
int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定右侧边界
            left = mid + 1;
        }
    }
    // 最后要检查 right 越界的情况
    if (right < 0 || nums[right] != target)
        return -1;
    return right;
}
```

## 遇到**最大化最小值或最小化最大值**，就是二分查找
[410. 分割数组的最大值](https://leetcode-cn.com/problems/split-array-largest-sum/)
