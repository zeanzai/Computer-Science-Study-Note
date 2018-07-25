---
layout: post
title:  "装箱和拆箱问题详解"
date:   2018-06-06 04:42:27 +0800
author: zeanzai
tags: 
    - study
    - note
---

----------
# 1. 导引
## 1.1 问题引入
我们知道，Java中int变量存在于jvm的静态区中，在系统之中它的存在形式很简单，就是一个简单的内存块，里面放了一个具体的数字，而Integer则是一个具体的对象，里面不光有具体的数字，还有一些具体的操作方法。早期的Java版本中，要想对int的数据进行对象化操作时，就必须要先把int转化为Integer对象才能够进行操作。例如：在Java1.5中，集合（Collections）不能直接放入int类型的数据，因为集合中要求存放的必须是对象。所以，为了节省由原始类型转化为对象类型的人工成本，也为了迎合Java中一切都是对象的思想，Java后续的版本中就引入了自动拆箱和自动装箱的机制。

----------


# 2. 概念
## 2.1 基本数据类型和封装数据类型
基本数据类型主要有8种，他们分别是：

| 基本数据类型(primitive) | byte    | short    | int     | long  | float | double | char      | boolean |
| ----------------------- | ------- | -------- | ------- | ----- | ----- | ------ | --------- | ------- |
| 占用内存大小            | 1字节   | 2字节    | 4字节   | 8字节 | 4字节 | 8字节  | 1 字节    | 未知    |
| 默认值                  | (byte)0 | (short)0 | 0       | 0L    | 0.0f  | 0.0d   | \\u000    | false   |
| 封装数据类型(wrapper)   | Byte    | Short    | Integer | Long  | Float | Double | Character | Boolean |
二者之间的区别：
| 基本数据类型                                                 | 封装数据类型                                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1.基本类型<br /> 2.是面向机器底层的数据结构，不需要实例化<br /> 3. 在内存中就是一个简单的变量，没有具体的操作方法 | 1.引用类型<br /> 2. 是Java对象，**可能**需要进行实例化<br /> 3. 有具体的操作方法 |

## 2.2 拆箱和装箱
装箱就是将基本数据类型转化为封装数据类型的过程，其主要使用了valueOf()函数将对象的值转化为封装类型的对象；同时，拆箱过程中，就使用intValue()、doubleValue()等函数，将对象的值转化为基本数据类型的值。

----------


# 3. 适用场景与不适用场景
## 3.1 适用场景
在Java1.5之前都需要手动的进行基本数据类型到封装类型之间的转化，Java1.5之后就可以实现自动的拆箱和装箱操作了。
### 3.1.1 赋值
自动拆箱和自动装箱的一个应用场景就是赋值操作。
如下面代码：
``` java
//before autoboxing
Integer iObject = Integer.valueOf(3);
Int iPrimitive = iObject.intValue()
 
//after java5
Integer iObject = 3; //autobxing - primitive to wrapper conversion
int iPrimitive = iObject; //unboxing - object to primitive conversion
```
Java编译器帮我们自动完成装箱和拆箱操作。
### 3.1.2 方法调用
另外一个应用场景就是方法调用，在方法调用的过程中，方法会根据具体的参数类型来进行拆箱和装箱操作。如下所示：
``` java 
public static Integer show(Integer iParam){
   System.out.println("autoboxing example - method invocation i: " + iParam);
   return iParam;
}
 
//autoboxing and unboxing in method invocation
show(3); //autoboxing
int result = show(3); //unboxing because return type of method is Integer
```
# 3.2 不适用场景
## 3.2.1 重载
什么情况下不适用，就是重载场景中。先看一个场景：
``` java
public void test(int num){
    System.out.println("method with primitive argument");
 
}
 
public void test(Integer num){
    System.out.println("method with wrapper argument");
 
}
 
//calling overloaded method
AutoboxingTest autoTest = new AutoboxingTest();
int value = 3;
autoTest.test(value); //no autoboxing 
Integer iValue = value;
autoTest.test(iValue); //no autoboxing
```
<pre>
输出:
method with primitive argument
method with wrapper argument
</pre>
在重载的过程中，如果多个同名函数中，一个使用基本数据类型作为参数，一个使用封装数据类型作为参数，那么在使用过程中不会发生重载，传递什么类型的参数，就使用什么类型的函数。
## 3.2.2 对象之间的比较
这是一个比较容易出错的地方，”==“可以用于原始值进行比较，也可以用于对象进行比较，当用于对象与对象之间比较时，比较的不是对象代表的值，而是检查两个对象是否是同一对象，这个比较过程中没有自动装箱发生。进行对象值比较不应该使用”==“，而应该使用对象对应的equals方法。看一个能说明问题的例子。
``` java 
public class AutoboxingTest {
 
    public static void main(String args[]) {
 
        // Example 1: == comparison pure primitive – no autoboxing
        int i1 = 1;
        int i2 = 1;
        System.out.println("i1==i2 : " + (i1 == i2)); // true
 
        // Example 2: equality operator mixing object and primitive
        Integer num1 = 1; // autoboxing
        int num2 = 1;
        System.out.println("num1 == num2 : " + (num1 == num2)); // true
 
        // Example 3: special case - arises due to autoboxing in Java
        Integer obj1 = 1; // autoboxing will call Integer.valueOf()
        Integer obj2 = 1; // same call to Integer.valueOf() will return same
                            // cached Object
 
        System.out.println("obj1 == obj2 : " + (obj1 == obj2)); // true
 
        // Example 4: equality operator - pure object comparison
        Integer one = new Integer(1); // no autoboxing
        Integer anotherOne = new Integer(1);
        System.out.println("one == anotherOne : " + (one == anotherOne)); // false
 
    }
 
}
```
<pre>
输出：
i1==i2 : true
num1 == num2 : true
obj1 == obj2 : true
one == anotherOne : false
</pre>
值得注意的是第三个小例子，这是一种极端情况。obj1和obj2的初始化都发生了自动装箱操作。但是处于节省内存的考虑，JVM会缓存-128到127的Integer对象。因为obj1和obj2实际上是同一个对象。所以使用”==“比较返回true。

----------


# 4. 使用中需要注意的问题
1. 在程序中，如果写了需要不断拆箱和装箱的操作代码，会造成额外的系统开销，增加垃圾回收的压力
2. 在自动拆箱和自动装箱过程中要注意默认值的问题（在拆箱过程中，如果使用了未初始化的对象，执行obj.xxxValue就会报NullPointerException异常）

----------


# 5. 总结
Java中的拆箱和装箱机制便利了程序开发人员，减少了程序开发人员的工作量，使得基本数据类型和封装数据类型之间有一个高效快捷的桥梁。但是凡事都有两面性，在使用过程中一定要注意初始值问题和适用场景问题，切不可以随意任意使用。















