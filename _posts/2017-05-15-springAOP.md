---
layout: post
title:  "spring之AOP基础"
date:   2017-04-23 00:06:05
categories: 技术
tags: spring
excerpt: spring之AOP基础
author: nivelle
---


* content
{:toc}

---


###  spring 之AOP原理基础

####  概述

采用动态代理机制和字节码生成技术，动态代理机制和字节码生成都是在运行期间为目标对象生成一个代理对象，而将横切逻辑织入到代理对象，系统最终使用的是织入了横切逻辑的代理对象，不是真正的目标对象。


######  AOP词汇

- Joinpoint
  
  在系统运行之前，AOP的功能模块都需要织入到OOP的功能模块中。所以，要进行这种织入过程，我们需要知道在系统的那些执行点上进行织入操作，这些将要在其之上进行织入操作的系统执行点就称之为Joinpoint.

  基本上，程序执行过程的任何时点都可以作为横切逻辑的织入点，这些都是Joinpoint.
  
  **常见Jpinpoint**:
  
  方法调用：当方法被调用的时候所处的程序执行点。
  
  方法调用执行：某个方法内部执行开始时点，应该与方法调用类型的Joinpoint进行区分。
  
  方法调用是在调用对象上的执行点，而方法执行则是在被调用到的方法逻辑执行的时点。对于同一对象，方法调用先于方法执行。
  
  构造方法调用，构造方法执行，字段设置，字段获取，异常处理执行，
  
- Pointcut

  Pointcut概念代表的是Joinpoint的表述方式。将横切逻辑织入当前系统的过程中，需要参照Pointcut规定的Joinpoint信息，才可以知道应该在系统的那些Joinponit上织入横切逻辑。
  
  Pointcut指定了系统中符合条件的一组Joinpoint。
  
  **ponitcut的表述方式**：
    - 直接指定Joinpoint所在方法名称：只限于支持方法级别的Joinpoint的AOP框架。
    - 正则表达式：利用表达式，来归纳表述需要符合某种条件的多组Joinpoint。
    - 使用特定的Ponitcut表述语言：AspectJ使用这种方式来指定Pointcut,它提供了一种类似正则表达式的针对Pointcut的表述语言

-  Advice
  
   Advice是一切横切关注点逻辑的载体，它代表将会植入到Joinpoint的横切逻辑。如果将Aspect比作OOP中的class,那么Advice就相当于Class中的Method.

    - Before Advice

      在JoinPoint指定位置之前执行Advice. 如果当前Before Advice 织入到方法执行类型的JoinPoint，那么这个Before Advice就会先于方法执行而执行。

      通常在这里做一些系统的初始化工作。
 
    - After Advice 

      在连接点之后执行的advice类型。
      
      - after returning advice 

        只有当前joinpoint正常流程执行完成后，它才会执行。比如方法执行正常返回没有异常抛出。

      - after throw advice 

        只有在当前joinpoint 执行过程中抛出异常的情况下，才会执行。比如某个joinpoint抛出异常而没有正常返回。

      - after advice 

        该类型advice不管joinpoint处执行流程是正常终了还是抛出异常都会执行，就像java中的finally一样。

   -  Around advice
     
     对附加其上的Joinpoint进行包裹，可以在Joinpoint之前和之后都指定相应的逻辑，甚至于中断或者忽略Joinpoint处原来程序流程的执行。

     Servlet规范提供的Filter功能，其实就是Around Advice的一种体现。
     
   - Introduction
   
     introduction不是根据横切逻辑在Joinpoint处的执行时机来区分的，而是根据它可以完成的功能而区别于其他Advice类型。
     
     AspectJ采用静态织入的形式，那么对象在使用的时候，Intruduction逻辑已经是编译织入完成的。

    
     ![image](http://7xpuj1.com1.z0.glb.clouddn.com/advice.png)
  - Aspect
  
    Aspect 是对系统中横切关注点逻辑进行模块化封装的AOP概念实体。通常，Aspect可以包含多个Poincut以及相关的Advice定义。
    
  **aspectJ风格定的Aspect申明：**

   ![image](http://7xpuj1.com1.z0.glb.clouddn.com/aspect.png)

 - 织入和织入器
 
   经过织入，以Aspect模块化的横切关注点集成到OOP的现存系统中。
   
   SpringAOP 使用一组类来完成最终的织入操作，ProxyFactory类则是Spring AOP 中最通用的织入器。

**总结：**

![image](http://7xpuj1.com1.z0.glb.clouddn.com/Aop%E5%9C%BA%E6%99%AF.png)

---

#### 实现方式

###### 代理模式

![image](http://7xpuj1.com1.z0.glb.clouddn.com/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F.png)

---

- ISubject:该接口是对被访问者或者被访问资源的抽象。
- SubjectImpl:被访问者或者被访问资源的具体实现。
- SubjectProxy:被访问者或者被访问资源的代理实现类，该类持有一个ISubject接口的具体实例。


SubjectImpl 和SubjectProxy都实现了相同的接口ISubject，而SubjectProxy内部持有SubjectImpl的引用。当Client通过request()请求服务的时候，SubjectProxy将转发给SubjectImpl。


####  动态代理

  JDK1.3之后引入了一种称为动态代理的机制。使用该机制我们可以为指定的接口在系统运行期间动态生成代理对象，从而帮助我们走出最初使用静态代理实现AOP的窘境。
  
  动态代理机制实现主要由一个类和一个接口组成。java.lang.reflect.Proxy类和Java.lang.reflect.InvocationHandler接口。
  
  
  不管几个目标对象，只要横切逻辑一样，所以只需要实现一个InvocationHandler就可以了：
  
```
public class RequestCtrlInvocationHandler implements InvocationHandler
{
   private Object target;
   
   public RequestCtrlInvocationHandler(Object target)
   {
       target=target;
   }
   
   public Object invoke(Object proxy,Method method,Object[] args) throws Throwable
   {
     if(method.getName().equals("request"))
     {
        TimeOfDay startTime = new TimeOfDay(0,0,0); 
        TimeOfDay endtTime = new TimeOfDay(5,59,59); 
        if(currentTime.isAfter(startTime)&&currentTime.isBefore(endtTime))
        {
            return null;
        }
        return method.invoke(target,args);
     }
     return null;
   }
}

然后使用proxy类，根据RequestCtrlInvocationHandler逻辑，为ISubject 和 IRequestable两种类型生成相应的代理对象实例：

Isuubject subject =(ISubject)Proxy.newProxyInstance(ProxyRunner.class.getClassLoader(),new Class[]{ISubject.class},new RequestCtrlInvocationHandler(new SubjectImpl()));
subject.request();

IRequestable requestable = (IRequestable)proxy.newProxyInstance(ProxyRunner.class.getClassLoader(),new Class[]{Irequestable.class},new RequestCtrlInvocationHandler(new requestableImpl()));
requestable.request();

```
当Proxy动态生成的代理对象上相应的接口方法被调用时，对应的InvocationHandler就会拦截相应的方法调用，并进行相应处理。

InvocationHandler就是我们实现横切逻辑的地方，他是横切逻辑的载体，作用跟Advice是一样的。所以，可以根据Advice的类型，分化出不同Advice类型的程序结构。


####  动态字节码生成

原理：我们可以对目标对象进行继承扩展，为其生成相应子类，子类可以通过复写来拓展父类的行为，将横切逻辑放到子类，系统目标使用子类即可。

要对类进行拓展，首先要实现一个net.sf.cglib.proxy.Callback.不过更多的时候，我们会直接使用net.sf.cglib.proxy.MethodInterceptor接口（拓展了Callback接口）

目标类：
```
public class Requestable{
  public void request(){
    system.out.println();
  }
}
```

```
public class RequestCtrlCallback implements MethodInterceptor
{
  public Object intercept(Object object,Method method,Object[] args,MethodProxy proxy) throws Throwable
  {
     if(method.getName().equals("request"))
     {
        TimeOfDay startTime = new TimeOfDay(0,0,0); 
        TimeOfDay endtTime = new TimeOfDay(5,59,59); 
        if(currentTime.isAfter(startTime)&&currentTime.isBefore(endtTime))
        {
            return null;
        }
        return method.invoke(target,args);
     }
     return null;
  }

}


```

这样，RequestCtrlCallback就实现类对request()方法请求进行访问控制的逻辑，现在我们要通过CGLB的Enhancer为目标对象动态地生成一个子类，并将RequestCtrlCallback中的横切逻辑附加到该子类中：

```
Enhancer enhancer = new Enhancer();
enhancer.setSuperclass(requestable.class);
enhancer.setCallback(new RequestCtrlCallback());

Requestable proxy = (Requestable)rnhancer.create();
proxy.request();


```
通过为enhancer指定需要生成的子类对应的弗雷，以及Callback实现，enhancer最终尾门生成了代理对象实例。

---
内容来自《Spring 揭秘》 作者：王福强
