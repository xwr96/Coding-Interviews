## 单例模式
```Java
单例模式第一版：
 
public class Singleton {
    private Singleton() {}  //私有构造函数
    private static Singleton instance = null;  //单例对象
    //静态工厂方法
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
### 代码解析
1.要想让一个类只能构建一个对象，自然不能让它随便去做new操作，因此Signleton的构造方法是私有的。


2.instance是Singleton类的静态成员，也是我们的单例对象。它的初始值可以写成Null，也可以写成new Singleton()。至于其中的区别后来会做解释。



3.getInstance是获取单例对象的方法。



如果单例初始值是null，还未构建，则构建单例对象并返回。这个写法属于单例模式当中的懒汉模式。


如果单例对象一开始就被new Singleton()主动构建，则不再需要判空操作，这种写法属于饿汉模式。


这两个名字很形象：饿汉主动找食物吃，懒汉躺在地上等着人喂。但是上边单列不是线程安全的单列！


为什么说刚才的代码不是线程安全呢？


假设Singleton类刚刚被初始化，instance对象还是空，这时候两个线程同时访问getInstance方法：


因为Instance是空，所以两个线程同时通过了条件判断，开始执行new操作：


这样一来，显然instance被构建了两次。让我们对代码做一下修改：
```Java
单例模式第二版：
 
public class Singleton {
   private Singleton() {}  //私有构造函数
   private static Singleton instance = null;  //单例对象
   //静态工厂方法
   public static Singleton getInstance() {
        if (instance == null) {      //双重检测机制
         synchronized (Singleton.class){  //同步锁
           if (instance == null) {     //双重检测机制
             instance = new Singleton();
               }
            }
         }
        return instance;
    }
}
 
为什么这样写呢？我们来解释几个关键点：
 
1.为了防止new Singleton被执行多次，因此在new操作之前加上Synchronized 同步锁，锁住整个类（注意，这里不能使用对象锁）。
 
2.进入Synchronized 临界区以后，还要再做一次判空。因为当两个线程同时访问的时候，线程A构建完对象，线程B也已经通过了最初的判空验证，不做第二次判空的话，线程B还是会再次构建instance对象。
```



```Java
如何避免这一情况呢？我们需要在instance对象前面增加一个修饰符volatile。
 
 
单例模式第三版：
 
public class Singleton {
    private Singleton() {}  //私有构造函数
    private volatile static Singleton instance = null;  //单例对象
    //静态工厂方法
    public static Singleton getInstance() {
          if (instance == null) {      //双重检测机制
         synchronized (Singleton.class){  //同步锁
           if (instance == null) {     //双重检测机制
             instance = new Singleton();
                }
             }
          }
          return instance;
      }
}
```
用最简单的方式理解，volatile修饰符阻止了变量访问前后的指令重排，保证了指令执行顺序！
经过volatile的修饰，当线程A执行instance = new Singleton的时候，JVM执行顺序是什么样？始终保证是下面的顺序：


memory =allocate();    //1：分配对象的内存空间 
ctorInstance(memory);  //2：初始化对象 

instance =memory;     //3：设置instance指向刚分配的内存地址 


如此在线程B看来，instance对象的引用要么指向null，要么指向一个初始化完毕的Instance，而不会出现某个中间态，保证了安全。
