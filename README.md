# Coding-Interviews
牛客网《剑指offer》答案和刷题心得
## 需要自己创建数据结构或是数据类型时的技巧
**要是构造函数有参数时，直接在类名外面创造对象，然后在参数里面直接`this.xx=xx`**

要是类没有参数时，直接在类外面创建对象，然后在里面调用就行。

```java
Map<Integer,Integer> dict;
    List<Integer> list;
    Random r=new Random();
    /** Initialize your data structure here. */
    public RandomizedSet() {
        dict=new HashMap<>();
        list=new ArrayList<>();
    }
```


## Java运算符优先级

优先级记忆方法：单目乘除为关系，逻辑三目后赋值

## 易错点
- 新建一个数值：new int[]{ , , }; **需要用大括号**
- 数组排序是Arrays.sort(nums)，List排序是collection.sort();
- 把数组转换为List需要用到API：Arrays.asList()
- 数组添加值不能用add方法，而是直接等号赋值。
- String类型只能通过"+"号来添加新的字符串，因为string类型是不可变类型。如果想要通过append()方法就需要把**String类型变为StringBuilder类型**，再通过方法 **.toString()** 转换为string类型。
- 在Integer类中有静态方法**toBinaryString**方法，此方法返回int变量的二进制表示的字符串。同理，Integer类中也提供了**toHexString（int i）方法和toOctalString**方法来分别返回int变量的16进制表示和8进制表示字符串。
- n&(n-1) 这个操作是算法中常见的，作用是消除数字 n 的二进制表示中的最后一个 1。
![图片](https://labuladong.gitee.io/algo/pictures/%E4%BD%8D%E6%93%8D%E4%BD%9C/1.png)
- **不需要额外空间的方法，就往位运算上想**
- java 对于找出连续数出现的各种奇怪的场景，数组桶永远是好的选择,即重新再new一个数组。
- 

### 异或规律
交换律：a ^ b ^ c <=> a ^ c ^ b

任何数于0异或为任何数 0 ^ n => n

相同的数异或为0: n ^ n => 0

var a = [2,3,2,4,4]

2 ^ 3 ^ 2 ^ 4 ^ 4等价于 2 ^ 2 ^ 4 ^ 4 ^ 3 => 0 ^ 0 ^3 => 3（对应LeetCode中136：只出现一次的数字）
