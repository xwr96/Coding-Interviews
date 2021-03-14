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
