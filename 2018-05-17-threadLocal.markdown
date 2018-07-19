---
layout: post
title: ThreadLocal探索
date: 2018-05-17 00:00:00
tags: 并发编程
---
平时用过几次threadlocal，感觉类似map，然后就引发了一个疑问：会不会导致内存溢出的问题？在此记录下搜索的答案。

弱引用的原文链接：
[Java弱引用(WeakReference)的理解与使用][1]
[理解Java中的弱引用][2]

##什么是对象可到达和对象不可到达状态
原文链接：[谈谈java中的WeakReference][3]

```
    A a = new A();
    B b = new B(a);
```

B的默认构造函数上是需要一个A的实例作为参数的，那么这个时候 A和B就产生了依赖，也可以说a和b产生了依赖。a是对象A的引用，b是对象B的引用，对象B同时还依赖对象A，<font color="red">那么这个时候我们认为从对象B是可以到达对象A的。</font>

```
    A a = new A();
    B b = new B(a);
    a = null;
```

A对象的引用a置空了，a不再指向对象A的地址，<font color="red">我们都知道当一个对象不再被其他对象引用的时候，是会被GC回收的，很显然及时a=null，那么A对象也是不可能被回收的，因为B依然依赖与A，在这个时候，造成了内存泄漏！</font>

```
    A a = new A();
    WeakReference b = new WeakReference(a);
    a = null;
```

<font color="red">当 a=null ，这个时候A只被弱引用依赖，那么GC会立刻回收A这个对象，这就是弱引用的好处！他可以在你对对象结构和拓扑不是很清晰的情况下，帮助你合理的释放对象，造成不必要的内存泄漏！</font>

## 弱对象的使用场景
原文链接：[对WeakReference的理解][4]
如果你想写一个 Java 程序，观察某对象什么时候会被垃圾收集的执行绪清除，你必须要用一个 reference 记住此对象，以便随时观察，但是却因此造成此对象的 reference 数目一直无法为零， 使得对象无法被清除。不过，现在有了 Weak Reference 之后，这就可以迎刃而解了。如果你希望能随时取得某对象的信息，但又不想影响此对象的垃圾收集，那么你应该用 Weak Reference 来记住此对象，而不是用一般的 reference。

```
    WeakReference wr=new WeakReference(obj);
    obj=null;//等待一段时间，obj对象就会被垃圾回收  
    if(wr.get()==null){  
        System.out.println("obj 已经被清除了");  
    }else{  
        System.out.println("obj尚未被清除");
    }
```

在此例中，透过 get() 可以取得此 Reference 的所指到的对象，如果传出值为 null 的话，代表此对象已经被清除。这类的技巧，在设计Optimizer或Debugger这类的程序时常会用到，因为这类程序需要取得某对象的信息，但是不可以影响此对象的垃圾收集。

## 深入分析 ThreadLocal 内存泄漏问题
原文链接：
[深入分析 ThreadLocal 内存泄漏问题][5]
[ThreadLocal可能引起的内存泄露][6]


  [1]: https://blog.csdn.net/zmx729618/article/details/54093532
  [2]: https://droidyue.com/blog/2014/10/12/understanding-weakreference-in-java/index.html
  [3]: https://blog.csdn.net/matrix_xu/article/details/8424038
  [4]: https://blog.csdn.net/sh15285118586/article/details/46934679
  [5]: http://www.importnew.com/22039.html
  [6]: http://www.cnblogs.com/onlywujun/p/3524675.html