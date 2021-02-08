### 滑动窗口概念

滑动窗口叫做「虫取法」，我觉得非常生动形象。因为滑动窗口的两个指针移动的过程和虫子爬动的过程非常像：前脚不动，把后脚移动过来；后脚不动，把前脚向前移动。

分享俩个**滑动窗口的模板**，能解决大多数的滑动窗口问题，一般用第一个模板：
```Java
void slideWindow(String s, String t) {
        HashMap<Character ,Integer> need=new HashMap<>();
        HashMap<Character,Integer> window=new HashMap<>();
        for(char c:t.toCharArray()){
            need.put(c,need.getOrDefault(c,0)+1);
        }
        int left=0,right=0;
        int valid=0;
        while(right<s.length()){
            // r 是将移入窗口的字符
            char r=s.charAt(right);
             // 右移窗口
            right++;
            
            // 进行窗口内数据的一系列更新
            
            ...
            
           // 判断左侧窗口是否要收缩
            while (window needs shrink) {
                // l 是将移出窗口的字符
                char l = s[left];
                // 左移窗口
                left++;
                // 进行窗口内数据的一系列更新
                
                ...
            }
        
        }
    }

```

```Java
public int findSubArray(nums) {
        
        int len=nums.length;//数组或字符串长度
        int left=0,right=0; //双指针，表示当前遍历的区间[left, right]，闭区间
        
        int sums=0；//用于统计子数组/子区间是否有效，根据题目可能会改成求和/计数
        int maxLength = 0 //保存最大的满足题目要求的子数组/子串长度

        while(right<len){ //当右边的指针没有搜索到数组/字符串的结尾
            sums=sums+array[right];//增加当前右边指针的数字/字符的求和/计数
            while(区间[left, right]不符合题意){//此时需要一直移动左指针，直至找到一个符合题意的区间
                sums=sums-array[left];// 移动左指针前需要从counter中减少left位置字符的求和/计数
                left++;//真正的移动左指针，注意不能跟上面一行代码写反
            }
            //到内层的while结束时，我们找到了一个符合题意要求的子数组/子串
            maxLength=Math.max(maxLength,right-left+1);
            right++; //移动右指针，去探索新的区间
        }
        return maxLength;//找到符合题意的子数组/子字符串长度
    }
}

```

滑动窗口中用到了左右两个指针，它们移动的思路是：以右指针作为驱动，拖着左指针向前走。右指针每次只移动一步，而左指针在内部 while 循环中每次可能移动多步。右指针是主动前移，探索未知的新区域；左指针是被迫移动，负责寻找满足题意的区间。

模板的整体思想是：

- 1、定义两个指针 left 和 right 分别指向区间的开头和结尾，注意是闭区间；定义 sums 用来统计该区间内的各个字符出现次数；
- 2、第一重 while 循环是为了判断 right 指针的位置是否超出了数组边界；当 right 每次到了新位置，需要增加 right 指针的求和/计数；
- 3、第二重 while 循环是让 left 指针向右移动到 [left, right] 区间符合题意的位置；当 left 每次移动到了新位置，需要减少 left 指针的求和/计数；
- 4、在第二重 while 循环之后，成功找到了一个符合题意的 [left, right] 区间，题目要求最大的区间长度，因此更新 res 为 max(res, 当前区间的长度) 。
- 5、right 指针每次向右移动一步，开始探索新的区间。

模板中的 sums 需要根据题目意思具体去修改，本题是求和题目因此把sums 定义成整数用于求和；如果是计数题目，就需要改成字典用于计数。当左右指针发生变化的时候，都需要更新 sums 。

### 例题分析

[leetcode-1208. 尽可能使字符串相等](https://leetcode-cn.com/problems/get-equal-substrings-within-budget/)

#### 题目描述
给你两个长度相同的字符串，s 和 t。

将 s 中的第 i 个字符变到 t 中的第 i 个字符需要 |s[i] - t[i]| 的开销（开销可能为 0），也就是两个字符的 ASCII 码值的差的绝对值。

用于变更字符串的最大预算是 maxCost。在转化字符串时，总开销应当小于等于该预算，这也意味着字符串的转化可能是不完全的。

如果你可以将 s 的子字符串转化为它在 t 中对应的子字符串，则返回可以转化的最大长度。

如果 s 中没有子字符串可以转化成 t 中对应的子字符串，则返回 0。


示例 1：

输入：s = "abcd", t = "bcdf", cost = 3
输出：3
解释：s 中的 "abc" 可以变为 "bcd"。开销为 3，所以最大长度为 3。
示例 2：

输入：s = "abcd", t = "cdef", cost = 3
输出：1
解释：s 中的任一字符要想变成 t 中对应的字符，其开销都是 2。因此，最大长度为 1。
示例 3：

输入：s = "abcd", t = "acde", cost = 0
输出：1
解释：你无法作出任何改动，所以最大长度为 1。

#### java实现

```Java
class Solution {
    public int equalSubstring(String s, String t, int maxCost) {
        char[] array=s.toCharArray();
        char[] array2=t.toCharArray();
        int len=array.length;
        int left=0,right=0;
        for(int i=0;i<len;i++){
            array[i]=(char)Math.abs(array[i]-array2[i]);
        }
        int res=0,maxLength=0;
        while(right<len){
            res=res+array[right];
            while(res>maxCost){
                res=res-array[left];
                left++;
            }
            maxLength=Math.max(maxLength,right-left+1);
            right++;
        }
        return maxLength;
    }
}
```
[leetcode-424：替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

