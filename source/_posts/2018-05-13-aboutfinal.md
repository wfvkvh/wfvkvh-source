---
layout: post_layout
title: 「并发」一个由final引起的低级错误引发的思考
time:   2018年05月13日 星期日
location: 北京
pulished: true
excerpt_separator: "```"
---

今天在看java.util.Collections集合工具类中的synchronizedMap()时，看到其内部静态类Synchronized构造方法上迷糊了一下

<!--more-->

```java
    private static class SynchronizedMap<K,V>
           implements Map<K,V>, Serializable {
           private static final long serialVersionUID = 1978198479659022715L;

           //静态成员map，未初始化
           private final Map<K,V> m;     // Backing Map
           //静态成员锁，未初始化
           final Object      mutex;        // Object on which to synchronize

           SynchronizedMap(Map<K,V> m) {
               //初始化
               this.m = Objects.requireNonNull(m);
               //初始化 指向当前类的实例
               mutex = this;
           }

           SynchronizedMap(Map<K,V> m, Object mutex) {
               //初始化 引用指向传入的map
               this.m = m;
               //初始化，引用指向传入的锁对象
               this.mutex = mutex;
           }
           //省略
    }
```

之前对final的意义一知半解，在网上哪里看过说是___final不能在构造器中(错)___，所以看到上面的代码第一反应，怎么在构造器中为final变量赋值了呢。

研究了半天，final修饰不可变变量，只能被初始化一次。
> 就是说声明的时候必须初始化，如果声明的时候没有初始化，必须在其他地方初始化。
如果在类中声明的时候没有初始化，编辑器会报错。但是当你之后在构造器中初始化了，它便不会再报错了。

```java
    //必须被初始化，而且初始化之后不能再被赋值
   public class TestFinal {
       public final int i;

       public TestFinal(int i) {
           this.i = i;
       }

       public  void test() {
           //再这样赋值是不行的
           this.i = 3;
       }
   }
```

## final的基本用法

1. 修饰类
    final修饰类，表明此类不能被继承，final类中的成员变量可以根据需要设为final，但是要注意final类中的所有成员方法都会被隐式地指定为final方法。
    > 在使用final修饰类的时候，要注意谨慎选择，除非这个类真的在以后不会用来继承或者出于安全的考虑，尽量不要将类设计为final类。
2. 修饰方法
    《Java编程思想》
   “使用final方法的原因有两个。第一个原因是把方法锁定，以防任何继承类修改它的含义；第二个原因是效率。在早期的Java实现版本中，会将final方法转为内嵌调用。但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升。在最近的Java版本中，不需要使用final方法进行这些优化了。“

    因此，如果只有在想明确禁止 该方法在子类中被覆盖的情况下才将方法设置为final的。

    > 类的private方法会隐式地被指定为final方法。

3. 修饰变量

    修饰变量是final用得最多的地方，也是本文接下来要重点阐述的内容。首先了解一下final变量的基本语法：

    > 对于一个final变量，如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。

    
## final 和 static

很多时候会容易把static和final关键字混淆
- static作用于成员变量用来表示只保存一份副本
- final的作用是用来保证变量不可变。
看下面这个例子：

```java
 public class Test {
     public static void main(String[] args)  {
         MyClass myClass1 = new MyClass();
         MyClass myClass2 = new MyClass();
         System.out.println(myClass1.i);
         System.out.println(myClass1.j);
         System.out.println(myClass2.i);
         System.out.println(myClass2.j);

     }
 }

 class MyClass {
     public final double i = Math.random();
     public static double j = Math.random();
 }
```

i的值是一直在变的，j的值是没变的

## final修饰的基本数据类型和对象

- final修饰的基本类型其值是不可更改的
- final修饰的对象，其引用是不变的，但是值可以改变

看如下代码：
```java
public class TestFinal {
    public static void main(String[] args) {
        FinalClass c = new FinalClass();
        final FinalClass d;
        d = c; //1
        c.k = 8;
        d.k = 3;

        c = new FinalClass(); //2
        c.k = 9;

        System.out.println("c.i:"+c.i);
        System.out.println("c.k:"+c.k);
        System.out.println("d.i:"+d.i);
        System.out.println("d.k:"+d.k);
    }
}
```

输出结果：

```
c.i:0.42658934545284133
c.k:9
d.i:0.6609599262688151
d.k:3
```

- 第一步c的引用赋值给被final修饰的d时，d的引用指向了c的对象，二者指向同一对象。d引用变为不可变，就是说d不能再被指向别的对象了，但是d内部的非final变量可以被重新赋值。
- 第二步c重新获得引用时，c是指向了一个新对象，跟原来的对象没关系了，所以它被final修饰的成员变量获取了新的值。

## final修饰的方法参数

```java
public class FinalClass {
    public  final double i = Math.random();
    public  static double j = Math.random();
    public int k = 0;

    public void finalObj(final StringBuilder sb){
        System.out.println(sb.toString());
    }

    public void finalBase(final int m){
        System.out.println("final int:" + m);
    }
}

public class TestFinal {
    public static void main(String[] args) {
        FinalClass c = new FinalClass();
        int demo = 3;
        StringBuilder sb = new StringBuilder();
        sb.append("Magic");

        c.finalObj(sb);
        System.out.println(sb.toString());

        c.finalBase(demo);
        System.out.println(demo);


    }
}
```

这个例子输出的结果是：
```
Magic world
Magic world
final int: 3
3
```
两个方法，方法参数都是final修饰，一个是对象，一个是基础数据类型。

```java
  public class FinalClass {
      public  final double i = Math.random();
      public  static double j = Math.random();
      public int k = 0;

      public void finalObj(StringBuilder sb){
          sb = new StringBuilder();
          System.out.println(sb.append(" world"));
      }

      public void finalBase(int m){
          System.out.println("final int:" + ++m);
      }
  }

  public class TestFinal {
      public static void main(String[] args) {
          FinalClass c = new FinalClass();
          int demo = 3;
          StringBuilder sb = new StringBuilder();
          sb.append("Magic");

          c.finalObj(sb);
          System.out.println(sb.toString());

          c.finalBase(demo);
          System.out.println(demo);


      }
  }
```

```
 world
Magic
final int:4
3
```
根据输出的结果我们可以确定：
- Java方法参数的传递是值传递，对象传递的是引用的值，基本类型传递的是本身的值
- 对象方法参数添加final并是传递的对象参数属性的值不被改变的原因。
    - 因为final的对象参数，被final修饰的也只是它的引用，而Java方法参数传递本身就是值传递，参数为对象时传递的就是对象引用的值，当然也可以说是传递的对象引用。
    - 未被final修饰对对象参数，在方法中被重新指向了别的引用，不会影响到传入的参数本身。
- 被final修饰的基本类型的方法参数，目的是为了在调用方法中，传入的参数值不被改变。

## final关键字的好处
下面总结了一些使用final关键字的好处

1. final关键字提高了性能。JVM和Java应用都会缓存final变量。
2. final变量可以安全的在多线程环境下进行共享，而不需要额外的同步开销。
3. 使用final关键字，JVM会对方法、变量及类进行优化。

## 关于final的重要知识点
1. final关键字可以用于成员变量、本地变量、方法以及类。
2. final成员变量必须在声明的时候初始化或者在构造器中初始化，否则就会报编译错误。
3. 你不能够对final变量再次赋值。
4. 本地变量必须在声明时赋值。
5. 在匿名类中所有变量都必须是final变量。
6. final方法不能被重写。
7. final类不能被继承。
8. final关键字不同于finally关键字，后者用于异常处理。
9. final关键字容易与finalize()方法搞混，后者是在Object类中定义的方法，是在垃圾回收之前被JVM调用的方法。
10. 接口中声明的所有变量本身是final的。
11. final和abstract这两个关键字是反相关的，final类就不可能是abstract的。
12. final方法在编译阶段绑定，称为静态绑定(static binding)。
13. 没有在声明时初始化final变量的称为空白final变量(blank final variable)，它们必须在构造器中初始化，或者调用this()初始化。不这么做的话，编译器会报错“final变量(变量名)需要进行初始化”。
14. 将类、方法、变量声明为final能够提高性能，这样JVM就有机会进行估计，然后优化。
15. 按照Java代码惯例，final变量就是常量，而且通常常量名要大写：
