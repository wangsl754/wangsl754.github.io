---
layout:     post
title:      "Java反射——获取构造器以及创建对象"
date:       2017-08-29 12:00:00
author:     "slwang"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - java
---

## 获取某一个类的所有构造器实例

 1. **明确操作的是哪一个类（严格的说是类编译后的字节码）**
 2. **获取构造器**

 Class类常用获取构造器的方法：
1.Class类的public Constructor<?> [] getConstructors()：该方法只能获取当前Class所表示类的public修饰的构造器
     2.Class类的public Constructor<?> [] getDeclaredConstructors()：该方法能获取所有的构造器，与访问权限无关
    3.Class类的public Constructor<?> [] getConstructors(Class<?> ... parameterType):只能获取单一公共类型的构造器
    4.Class类的public Constructor<?> [] getDeclaredConstructors(Class<?> ... parameterType):只能获取单一任意类型的构造器
                 parameterType表示构造器参数的Class类型
                 
 Constructor类：表示类的构造器类型，Constructor实例就是某一个类的某个构造器
 
##获取构造器创建实例
Constructor类：表示类的构造器类型，Constructor实例就是某一个类的某个构造器

常用方法：
public T newInstance(Object...initargs)
     参数；initargs表示调用构造器的实际参数
      返回；返回创建的实例。T表示Class类所表示的类型
如果一个类的构造器可以被直接访问，同时没有参数；那么可以直接使用Class类的
public T newInstance直接创建对象

代码示例：
1.ConstructorDemo.java

```java
package constuctor;

import java.lang.reflect.Constructor;

//使用反射获取构造器
public class ConstructorDemo {

	public static void main(String[] args) throws Exception {
		getOneConstructor();
		System.out.println("=============");
		getAllConstructor();

	}
	
	//获取单一构造器
	private static void getOneConstructor() throws Exception, Exception{
		//找到User的字节码
		Class<User> clz=User.class;
		
		//调用获取指定构造器的方法即可
		//如想`获取public User(String name)
		//应告诉程序获取带有一个String类型参数的构造器
		Constructor c=clz.getConstructor(String.class);
		System.out.println(c);
	}
	
	//获取所有构造器
	private static void getAllConstructor() throws Exception, SecurityException{
		//找到User的字节码
		Class<User> clz=User.class;
		
		//获取public修饰的构造器
		Constructor[] cs=clz.getConstructors();
		for(Constructor c: cs)
			System.out.println(c);
		
		System.out.println("=============");
		//获取任意类型的构造器
		cs=clz.getDeclaredConstructors();
		for(Constructor c: cs)
			System.out.println(c);
	}

}
class User{
	public User(){}
	public User(String age){}
	public User(String name,int age){}
	private User(String name,int age,String address){}
	
}
```

运行结果

```java
public constuctor.User(java.lang.String)
=============
public constuctor.User(java.lang.String,int)
public constuctor.User(java.lang.String)
public constuctor.User()
=============
private constuctor.User(java.lang.String,int,java.lang.String)
public constuctor.User(java.lang.String,int)
public constuctor.User(java.lang.String)
public constuctor.User()
```

2.ConstructorInvokeDemo.java

```java
package constuctor;

import java.lang.reflect.Constructor;

//使用反射调用构造器，创建对象
public class ConstructorInvokeDemo {

	public static void main(String[] args) throws Exception, Exception {
		//1.先找到被调用对象的字节码
		Class<Emp> clz=Emp.class;
		//2.找到被调用的指定的构造器
		Constructor<Emp> c=clz.getConstructor();
		//3.执行该构造器
		Emp e=c.newInstance();
		System.out.println(e);
		
		System.out.println("////////////////////////////////////////////////////////////////////////////////////////////////////");
		Emp e2=clz.newInstance();
		System.out.println(e2);
		
		//调用带参数的构造器
		System.out.println("////////////////////////////////////////////////////////////////////////////////////////////////////");
		Constructor<Emp> c2=clz.getConstructor(String.class);
		Emp e3=c2.newInstance("wang");
		System.out.println(e3);
		
		//调用私有构造器
		System.out.println("////////////////////////////////////////////////////////////////////////////////////////////////////");
		c=clz.getDeclaredConstructor(String.class,int.class,String.class);
		//Emp e4=c.newInstance("wang",23,"beijing");//java.lang.IllegalAccessException:  can not access a member of class constuctor.Emp with modifiers "private"
		//设置私有构造器可访问
		c.setAccessible(true);
		Emp e4=c.newInstance("wang",23,"beijing");
		System.out.println(e4);
	}

}

class Emp{
	public Emp(){
		System.out.println("...emp()...");
	}

	public Emp(String name){
		System.out.println("...emp()..."+name);

	}
	public Emp(String name,int age){
		System.out.println("...emp()..."+name+" "+age);

	}
	private Emp(String name,int age,String address){
		System.out.println("...emp()..." +name+" "+age+" "+address);

	}
	
}
```

运行结果

```java
...emp()...
constuctor.Emp@15db9742
////////////////////////////////////////////////////////////////////////////////////////////////////
...emp()...
constuctor.Emp@6d06d69c
////////////////////////////////////////////////////////////////////////////////////////////////////
...emp()...wang
constuctor.Emp@7852e922
////////////////////////////////////////////////////////////////////////////////////////////////////
...emp()...wang 23 beijing
constuctor.Emp@4e25154f
```



