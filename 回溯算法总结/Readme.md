### 概念
回溯法 采用试错的思想，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将取消上一步甚至是上几步的计算，再通过其它的可能的分步解答再次尝试寻找问题的答案。回溯法通常用最简单的递归方法来实现，在反复重复上述的步骤后可能出现两种情况：

找到一个可能存在的正确的答案；
在尝试了所有可能的分步方法后宣告该问题没有答案。

深度优先搜索 算法（英语：Depth-First-Search，DFS）是一种用于遍历或搜索树或图的算法。这个算法会 尽可能深 的搜索树的分支。当结点 v 的所在边都己被探寻过，搜索将 回溯 到发现结点 v 的那条边的起始结点。这一过程一直进行到已发现从源结点可达的所有结点为止。如果还存在未被发现的结点，则选择其中一个作为源结点并重复以上过程，整个进程反复进行直到所有结点都被访问为止。

简单来说，回溯算法用于搜索一个问题的所有的解 ，通过深度优先遍历的思想实现。

- 注意如果有**重复元素且**又要做排列或者子集的话我们一般**先排序**，方便后面剪枝
   `Arrays.sort(nums);`
 
 **例题**：[子集II](https://leetcode-cn.com/problems/subsets-ii/)

### 优化剪枝（一般在for循环里面）
分析搜索起点的上界进行剪枝，公式：把 i <= n 改成 i <= n - (k - path.size()) + 1 

### 回溯算法理解的“回溯”
可以想象搜索遍历的问题其实就像是做实验，每一次实验都用新的实验材料，那么做完了就废弃了。但是如果只使用一份材料，在做完一次以后，一定需要将它恢复成原样（就是这里「回溯」的意思），才可以做下一次尝试。

### 题目类型
##### 一、组合、切割、排列、子集、棋盘**
##### 二、Flood Fill（洪水泛滥问题）
##### 三、字符串中的回溯问题
##### 四、游戏问题

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



