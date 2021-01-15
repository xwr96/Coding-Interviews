### 概念


### 题目类型
**组合、切割、排列、子集、棋盘**
### 代码模板
```Java
void backTrace(参数：开始的Index，数组等){
  if(停止条件){
    收集结果；
    return；
  }
  for(集合元素){
    处理节点(一般是.add)；
    递归调用；
    回溯操作；
  }
} 
```


### 例题1:组合总和
**题目**:给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的数字可以无限制重复被选取。

说明：

所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 
示例 1：

输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
**代码解法：**
```Java
class Solution {
    List<List<Integer>> lists=new ArrayList<>();
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        
        if (candidates==null || candidates.length==0 || target<0){
            return lists;
        }

        List<Integer> list=new ArrayList<>();
        process(0,candidates,target,list);
        return lists;
    }

    private void process(int startIndex,int[] candidates,int target,List<Integer> list){
        if(target<0){
            return;
        }
        if(target==0){
            lists.add(new ArrayList<>(list));
        }else{
            for(int i=startIndex;i<candidates.length;i++){
                list.add(candidates[i]);
                process(i,candidates,target-candidates[i],list);
                list.remove(list.size()-1);
            }
        }
    }
    
}
```
### 题目2：求数组子集问题
**题目：** 给你一个整数数组 nums ，返回该数组所有可能的子集（幂集）。解集不能包含重复的子集。

 
示例 1：

输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
示例 2：

输入：nums = [0]
输出：[[],[0]]

**代码解法：**
```Java
class Solution {
    List<List<Integer>> lists=new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        
        if(nums==null || nums.length==0){
            return lists;
        }
        
        List<Integer> list=new ArrayList<>();
        backTrace(0,nums,list);
        return lists;
    }

    public void backTrace(int startIndex,int[] nums,List<Integer> list){
        
        lists.add(new ArrayList<>(list));
        
        for(int i=startIndex;i<nums.length;i++){
            list.add(nums[i]);
            backTrace(i+1,nums,list);
            list.remove(list.size()-1);
        }
    }
}

```



