---
layout:     post
title:      "Java自动装箱(AutoBoxing)与拆箱(Unboxing)"
subtitle:   " \"AutoBoxing and Unboxing\""
date:       2020-05-10 12:00:00
author:     "Eule"
header-img: "img/post-bg-2015.jpg"
comments: true
tags:
    - Java
---

@[TOC](【Java基础】Java中的自动拆箱与装箱)

# 目录大纲


## 一、什么是装箱？什么是拆箱？

**装箱**: 将基本数据类型用它们对应的包装类型包装起来;
**拆箱**:将包装类型转换为基本数据类型;

```java
  Integer x = 66; //自动装箱
  int y = x;// 自动拆箱
```
Java 为每种基本数据类型都提供了对应的引用类型，在Java SE5开始就提供了自动装箱和自动拆箱的特性
|基本数据类型|包装类型  |
|--|--|
|  int （4字节）|Interger  |
|byte（1字节）|Byte|
|short（2字节）|Short|
|long（8字节）|Long|
|float（4字节）|Float|
|double（8字节）|Double|
|char（2字节）|Character|
|boolean（取决于JVM）|Boolean|

## 二、解决了啥问题？
在Java环境中，基本数据类型不是对像，也就不可能是Object的子类，为了达成万物皆Object的大圆满，自然就出现了装箱和拆箱，将基本数据类型包装成一个具有对应包装类型的属性的对象，从而获得更多功能。如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509210809319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg2NDc1OA==,size_16,color_FFFFFF,t_70)


## 三、自动拆箱和自动装箱的原理

以Integer为例我们分析一下以下代码：

```java
package cn.codeowl.test.lang.Integer;

public class Test {

    public static void main(String[] args) {
        Integer x = 66; //自动装箱
        int y = x;//自动拆箱
    }
}

```
用命令 `javap -c Test`反编译class文件得到以下内容

```java
Compiled from "Test.java"
public class cn.codeowl.test.lang.Integer.Test {
  public cn.codeowl.test.lang.Integer.Test();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: bipush        66
       2: invokestatic  #2                  // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
       5: astore_1
       6: aload_1
       7: invokevirtual #3                  // Method java/lang/Integer.intValue:()I
      10: istore_2
      11: return
}
```
从上面的代码能看出，在装箱时调用了Integer的静态方法valueOf,在拆箱时调用Integer的实例方法intValue。同样的方法分析其他基本数据类型的装箱和拆箱，可以发现：装箱就是通过调用包装类型的静态方法valueOf实现的，拆箱就是通过调用包装类型实例的实例方法xxxValue(intValue、doubleValue、floatValue等)实现的

## 四、经常考察的点
### 关于IntegerCache对int 数据类型自动装箱的影响
 首先我们先看一段代码：
 

```java
package cn.codeowl;

public class Main {

    public static void main(String[] args) {
	    Integer n1 = 127;
	    Integer n2 = 127;
	    Integer n3 = 128;
	    Integer n4 = 128;

        System.out.println(n1==n2);
        System.out.println(n3==n4);
    }
}

```
在看一下运行结果

```java
true
false
```
是不是觉得很奇怪呢？输出的结果表明n1和n2是同一个对象，n3和n4指向的不是同一个对象。我们去探个究竟，因为是自动装箱按上文所说是调用了Integer的valueOf,下面这段代码是valueOf方法的具体实现

```java
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```
其中调用了IntegerCache接着追进去看一下实现

```java
    private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }
```
可见**默认情况**下在通过valueOf方法创建Integer对象时，当数值在[-128, 127]区间则直接返回**IntegerCache.cache**中已经存在的对象的引用，否则new 一个新的Integer对象。上面代码中的n1、n2刚好在这个区间，因此会直接返回cache中已经创建好的对象，所以n1和n2指向同一个对象，n3和n4分别指向不同的对象。
### Integer i = new Integer(xxx)和Integer i = xxx的区别
分析基本与上一个点一样，主要有两点

 1. 第一种不会触发自动装箱也就是不会调用valueOf来创建Integer对象；第二种会触发
 2. 由于valueOf创建对象引进了IntegerCache，所以第二种执行效率和资源占用上***一般情况***下优于第一种。
 ###  其他
	其他包装类型基本上也是相似的：
	1、Integer、Long、Short、Byte、Character这几个包装类型的valueOf方法的实现套路基本一致；
	2、Double、Float的valueOf方法实现类似；
	不难分析出原因：在某个区间内第一类的数值的个数是有限的适合设定缓存区间，第二类的数值是浮点数是无限的不适合设定缓存区间
	[参考](https://www.cnblogs.com/dolphin0520/p/3780005.html)



