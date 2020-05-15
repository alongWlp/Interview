# JVM相关面试题

**包括jvm内存模型，jvm线上问题排查**

**JVM面试总结**
## 基础知识面试总结
## 1、为什么使用消息队列？
分析：一个用消息队列的人，不知道为啥用，这就有点尴尬、没有复习这点，很容易被问蒙，然后就开始胡扯了。<br>
回答：这个问题，咱只答三个最主要的应用场景（不可否认还有掐的，但是只答三个主要的），即以下六个字：解耦、异步、削峰<br>
## 2、使用了消息队列会有什么缺点？
垃圾回收：释放垃圾占用的内存空间，防止内存泄露，有效的使用内存中可以使用的内存，对内存堆中已经死亡或者长时间没有使用的对象进行清除和回收。

## 问题：怎么确认是垃圾？

引用计数法：通过在对象头中分配一个空间，来保存改对象被引用的次数。如果改对象被其他的对象所引用，则引用计数加1，如果删除对改对象的引用，则引用次数减1，当改引用次数为0的时候，那么改对象就会被回收。

```java
String m = new String("jack"); // m是jack字符串的引用
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b43256d6-f71b-4c6c-bde2-850353381e2b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b43256d6-f71b-4c6c-bde2-850353381e2b/Untitled.png)

```java
m=null;//将m置为空，这时jack的引用次数就是0了，在引用计数里，意味着这块内存可以被回收了。
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c198973c-3d6f-421b-b1b3-7904d84280ad/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c198973c-3d6f-421b-b1b3-7904d84280ad/Untitled.png)

可达性分析算法：通过一些引用链（GC ROOTs)的对象做为起点，从这些节点向下探索，搜索走过的路径被称为(Reference Chain)，当一个对象到GC Roots没有任何引用链相连接的时候，则证明该对象是不可达的，就可以进行回收。

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60b5b60b-4a6e-4b6c-95a3-8d4d903b5ba7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60b5b60b-4a6e-4b6c-95a3-8d4d903b5ba7/Untitled.png)

在Java中，可以作为GC Root的对象包括以下4种

1.虚拟机栈中引用的对象。

2.方法区中静态属性引用的对象。

3.方法区中常量引用的对象。

4.本地方法栈中，JNI引用的对象。

jvm结构

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a7f0712-fc97-4136-a3e5-cdb52b960440/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a7f0712-fc97-4136-a3e5-cdb52b960440/Untitled.png)

## 垃圾回收算法：

标记清除算法：先把内存中需要回收的对象进行标记，然后进行回收。这样容易产生内存碎片。

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0145e9af-f95d-4c44-ab37-9d645582f68e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0145e9af-f95d-4c44-ab37-9d645582f68e/Untitled.png)

复制算法：将内存分为大小相等的两块，每次只使用其中的一块，当这一块内存用完了，就将还存活的对象复制到另一块，然后再把已使用过的内存空间一次清理掉。

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9ecb6671-c031-4d47-91f0-314506faa016/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9ecb6671-c031-4d47-91f0-314506faa016/Untitled.png)

分代回收算法，java堆被分为新生代和老年代。在新生代中又划分了三个区域：Eden空间，To Survivor空间，From Survivor空间。

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efb4f98d-5d82-4373-892f-20d608351734/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/efb4f98d-5d82-4373-892f-20d608351734/Untitled.png)

新创建的对象会被放在Eden区，当新生代发生GC后，会将Eden区和其中一个survivor区空间的内存复制到另一个survivor中，如果反复几次一直存活，此时对象会被移至老年代。

## 类加载机制

类的加载指的是，将类的.class二进制文件加载到内存中，将其放在运行时数据区的方法区内，然后在堆区上创建一个java.lang.Class对象，用来封装类在方法区内的数据结构。

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b2818d9-e25b-42bc-a7e4-3a8c5bc181b0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b2818d9-e25b-42bc-a7e4-3a8c5bc181b0/Untitled.png)

JVM的类加载主要是通过ClassLoader及其子类来完成的，类的层次和加载顺序如下图所示。

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7efc661d-3b42-4b08-960f-e74cc3afdff1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7efc661d-3b42-4b08-960f-e74cc3afdff1/Untitled.png)

1.Bootstarap ClassLoader 负责加载$JAVA_HOME中的jre/lib/rt.jar 里所有的class或Xbootclassoath选项指定的jar包。由C++实现，不是ClassLoader子类。

2.Extension ClassLoader 负责加载java平台中扩展功能的一些jar包，包括$JAVA_HOME中jre/lib/*.jar 或 -Djava.ext.dirs指定目录下的jar包。

3.App ClassLoader 负责加载classpath中指定的jar包及 Djava.class.path 所指定目录下的类和jar包。

4.Custom ClassLoader 通过java.lang.ClassLoader的子类自定义加载class，属于应用程序根据自身需要自定义的ClassLoader，如tomcat、jboss都会根据j2ee规范自行实现ClassLoader。

加载过程中会先检查类是否被已加载，检查顺序是自底向上，从Custom ClassLoader到BootStrap ClassLoader逐层检查，只要某个classloader已加载，就视为已加载此类，保证此类只所有ClassLoader加载一次。而加载的顺序是自顶向下，也就是由上层来逐层尝试加载此类。

## 基础知识面试总结


