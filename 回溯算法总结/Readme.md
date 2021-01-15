### 概念


### 代码模板
<pre name="code",class="java">
void backTrace(参数：开始的Index，数组等){
  
} 
</pre>


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




