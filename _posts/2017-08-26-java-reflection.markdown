---
layout:     post
title:      "Java反射——获取Class对象"
date:       2017-08-26 12:00:00
author:     "slwang"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - java
---
﻿
## 反射简介
能够分析类能力的程序称为反射，它允许运行中的Java程序获取自身的信息，并且可以操作类或对象的内部属性。反射功能及其强大，它可以用来：

 - 在运行中分析类的能力。
 - 在运行中查看对象，例如，编写一个toString方法工所有类使用。
 - 实现通用的数组操作代码。
 - 利用Method对象，这个对象很想c++中的函数指针。

反射是一种功能强大且复杂的机制，使用它的主要是工具构造者，而不是应用程序员。
简而言之，通过反射，我们可以在运行时获得程序或程序集中每一个类型的成员和成员的信息。
程序中一般的对象的类型都是在编译期就确定下来的，而Java反射机制可以动态地创建对象并调用其属性，这样的对象的类型在编译期是未知的。所以我们可以通过反射机制直接创建对象，即使这个对象的类型在编译期是未知的。
反射的核心是JVM在运行时才动态加载类或调用方法/访问属性，它不需要事先（写代码的时候或编译期）知道运行对象是谁。

## Class类
在程序运行期间，Java运行时系统始终为所有的对象维护一个被称为运行时的类型标志。这个信息跟踪着每个对象所属的类。虚拟机利用运行时类型信息选择相应的方法执行。然而，可以通过专门的Java类访问这些信息。保存这些信息的类被称为Class。
Class类封装了一个对象或接口的运行时状态，当装载类时，Class类型的对象自动创建。
Class 没有公共构造方法。Class对象是在加载类时由Java虚拟机以及通过调用类加载器中的defineClass方法自动构造的，因此不能显式地声明一个Class对象。
虚拟机为每种类型管理一个独一无二的Class对象。也就是说，每个类（型）都有一个Class对象。运行程序时，Java虚拟机(JVM)首先检查是否所要加载的类对应的Class对象是否已经加载。如果没有加载，JVM就会根据类名查找.class文件，并将其Class对象载入。
基本的 Java 类型（boolean、byte、char、short、int、long、float 和 double）和关键字 void 也都对应一个 Class 对象。
每个数组属于被映射为 Class 对象的一个类，所有具有相同元素类型和维数的数组都共享该 Class 对象。
一般某个类的Class对象被载入内存，它就用来创建这个类的所有对象。

## 利用反射获取Class对象

 1. **数据类型.class**
 这个方法可以直接获得与指定类关联的Class对象，而并不需要有该类的对象存在。
 2. **Class.forName(String className);根据类的权限定名构建Class对象**
 Class有一个著名的static方法forName：public static Class forName(String className) throws ClassNotFoundException
该方法可以根据字符串参数所指定的类名获取与该类关联的Class对象。如果该类还没有被装入，该方法会将该类装入JVM。
该方法声明抛出ClassNotFoundException异常。顾名思义，当该方法无法获取需要装入的类时（例如，在当前类路径中不存在这个类），就会抛出这个异常。
该方法通常用于在程序运行时根据类名动态的载入该类并获得与之对应的Class对象。
 3. **通过Object类的getClass方法来获取**
 java.lang.Object中定义有getClass方法：public final Class getClass()
所有Java对象都具备这个方法，该方法用于返回调用该方法的对象的所属类关联的Class对象

**代码示例：**
1.javaDemo.User.java

```java
package javaDemo;

public class User {
	private String name;
	private int age;
	
	public void test(){
		
	}

}
```

2.clazz.ClassinstaceDemo.java
```java
package clazz;

import javaDemo.User;

//获取Class类名
public class ClassinstaceDemo {

	public static void main(String[] args) throws Exception {
		
		//第一种方式：类名.class
		Class<User> clz1=User.class;
		
		//第二种方式：Class.forName(String className);
		Class<?> clz2=Class.forName("javaDemo.User");
		
		//第三种方式：getClass()方法
		User user=new User();
		Class clz3=user.getClass();
		System.out.println(clz1);
		System.out.println(clz2);
		System.out.println(clz3);
	}

}
```

3.运行结果
```
class javaDemo.User
class javaDemo.User
class javaDemo.User
```

