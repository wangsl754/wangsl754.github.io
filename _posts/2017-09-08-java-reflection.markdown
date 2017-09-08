---
layout:     post
title:      "java反射——获取及调用方法"
date:       2017-09-08 12:00:00
author:     "slwang"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - java
---


## 获取某一个类的方法

 1. **找到获取方法所在类的字节码对象**
 2. **找到需被获取的方法**

 Class类常用的方法：
1.public Method[] getMethods(); //获取自身以及继承过来的所有public方法
    2.public Method[] getDeclaredMethods();//获取自身所有的方法(不包括继承的,和访问权限无关)
    3.public Method getMethod(String name,Class<>...parameterTypes);//表示调用指定的方法
            参数：name:表示被调用的方法名
                    parameterTypes:表示被调用的方法的参数的Class类型
4.public Method getDeclaredMethod(String name,Class<>... parameterTypes)://表示调用指定的一个本类中的方法(不包括继承的)
    参数:
           methodName: 表示被调用方法的名字
           parameterTypes:表示被调用方法的参数的Class类型如String.class
 
## 使用反射调用方法

 1. **找到被调用方法所在的字节码**
 2. **获取到被调用的方法对象**
 3. **调用该方法**

在Method类中有方法：
public Object invoke（Object ob，Object...args):表示当前Method所表示的方法
          参数：
                  obj:表示被调用方法所属的底层对象
                  args:表示被调用方法时所传递的实际参数
          返回：
                  底层方法的返回结果
                  
***调用私有方法（切记）：
在调用私有方法之前:应该设置该方法为可访问的
又因为Method是AccessibleObject子类,所以Method中具有该方法.
sayGoodByeMethod.setAccessible(true);***

***使用反射调用静态方法:
public Object invoke(Object obj,Object... args);
如果底层方法是静态的，那么可以忽略指定的 obj参数。将obj参数设置为null即可。***

***使用反射调用可变参数的方法:
对于数组类型的引用类型的参数,底层会自动解包,为了解决该问题,我们使用Object的一维数组把实际参数包装起来.***

***(牢记)以后使用反射调用invoke方法,在传递实际参数的时候,无论是基本数据类型还是引用数据类型,也无论是可变参数类型,反正就是一切实际参数都包装在newObject[]{}中,就没问题。***

代码示例：

1.Methodome.java
```java
package methodome;

import java.lang.reflect.Method;

//获取User类中的方法
public class Methodome {

	public static void main(String[] args) throws Exception{
		//getAllMethod();
		getOneMethod();
	}
	
	//获取某一个方法
	public static void getOneMethod() throws Exception{
		//获取sayHello()
		Class clz=User.class;
		//只有通过方法签名才能找到唯一的方法
		//方法签名=方法名+参数列表（参数类型，参数个数，参数顺序）
		Method m=clz.getMethod("sayHello", null);
		System.out.println(m);
	}
	//获取所有的方法
	public static void getAllMethod(){
		Class clz=User.class;
		Method[] mt=clz.getMethods();
		for(Method m:mt){
			System.out.println(m);
		}
		
		System.out.println("............................................................................................................................");
		mt=clz.getDeclaredMethods();
		for(Method m:mt){
			System.out.println(m);
		}
	}

}

class User{
	public void sayHello(){
		System.out.println("sayHello().......");
	}
	
	public void sayHi(String name){
		System.out.println("sayHi().........."+name);
	}
	
	private void sayGoodbye(String name,int age){
		System.out.println("sayGoodbye()......."+name+age);
	}
}
```

2.MethodInvokeDemo.java

```java
package methodome;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;;
//方法的调用
public class MethodInvokeDemo {

	public static void main(String[] args) throws NoSuchMethodException, SecurityException, IllegalAccessException, IllegalArgumentException, InvocationTargetException, InstantiationException {
		//调用sayHi（String name）
		Class clz=Emp.class;
		Method sayHiMethod=clz.getMethod("sayHi", String.class);
		Object obj=clz.newInstance();//obj表示sayHi方法的底层所属对象
		Object res=sayHiMethod.invoke(obj, "wang");//wang表示sayHi的实际参数
		System.out.println(sayHiMethod);
		System.out.println(res);//res表示sayHi方法的返回结果 
		
		//调用私有sayGoodBye(String name,int age)
		Method sayGoodBye=clz.getDeclaredMethod("sayGoodBye", String.class,int.class);
		sayGoodBye.setAccessible(true);//将私有方法设置为可访问的
		res=sayGoodBye.invoke(obj, "wang",23);
		
	}

}

class Emp{
	public void sayHello(){
		System.out.println("sayHello().......");
	}
	
	public int sayHi(String name){
		System.out.println("sayHi().........."+name);
		return 1;
	}
	
	private void sayGoodBye(String name,int age){
		System.out.println("sayGoodbye()......."+name+age);
	}
}
```




