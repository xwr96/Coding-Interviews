## 自己构造HashMap需要实现的类
```Java
private class Pair{
        private int key;
        private int value;
        public Pair(int key,int value){
            this.key=key;
            this.value=value;
        }
        public int getKey(){
            return key;
        }
        public int getValue(){
            return value;
        }
        public int setValue(int value){
            this.value=value;
        }

    }
```

## 统计字母出现的次数
```Java
int[] count=new int[26];
for(char ch : s.toCharArray()){
        count[ch - 'a']++;
    }
```
## 单词字典序问题
从字典序入手，第一个字母靠前可以使整个字符串靠前 --->贪心

每次贪心要找到一个当前最靠前的字母 --->单调栈
字典序一般用双向队列实现：`LinkedList.getLast()>xx`
```Java
// 声明变量

for{    // 遍历字符串s
    // 初始化字母出现次数哈希表
}

for{    // 遍历字符串s
    if(该位置字母没有在栈中出现过){
        while(栈不为空 && 栈顶元素字典序列靠后 && 栈顶元素还有剩余出现次数){
            // 修改出现状态
            // 栈顶元素弹出
        }
        // 该位置的字母压栈
        // 修改出现状态
    }
    // 遍历过的字母出现次数减一
}

// 返回栈中元素

```
