---
layout: post
title:  Callable、Future和FutureTask浅析
date:   2017-12-21 00:06:05
categories: 多线程
tags: callable
excerpt: Callable、Future和FutureTask浅析
author: nivelle
---

### Callable<V>接口

我们先回顾一下java.lang.Runnable接口，就声明了run(),其返回值为void，当然就无法获取结果了。

```

public interface Runnable {  
    public abstract void run();  
}  

```
而Callable的接口定义如下:

```

public interface Callable<V> {   
      V   call()   throws Exception;   
}  


```

该接口声明了一个名称为call()的方法，同时这个方法可以有返回值V，也可以抛出异常。

无论是Runnable接口的实现类还是Callable接口的实现类，都可以被ThreadPoolExecutor或ScheduledThreadPoolExecutor执行，ThreadPoolExecutor或ScheduledThreadPoolExecutor都实现了ExcutorService接口，而因此Callable需要和Executor框架中的ExcutorService结合使用，我们先看看ExecutorService提供的方法：

```

<T> Future<T> submit(Callable<T> task);  
<T> Future<T> submit(Runnable task, T result);  
Future<?> submit(Runnable task);  


```

第一个方法：submit提交一个实现Callable接口的任务，并且返回封装了异步计算结果的Future。

第二个方法：submit提交一个实现Runnable接口的任务，并且指定了在调用Future的get方法时返回的result对象。

第三个方法：submit提交一个实现Runnable接口的任务，并且返回封装了异步计算结果的Future。

因此我们只要创建好我们的线程对象（实现Callable接口或者Runnable接口），然后通过上面3个方法提交给线程池去执行即可。还有点要注意的是，除了我们自己实现Callable对象外，我们还可以使用工厂类Executors来把一个Runnable对象包装成Callable对象。Executors工厂类提供的方法如下：


```
public static Callable<Object> callable(Runnable task)  
public static <T> Callable<T> callable(Runnable task, T result)  

```

### Future<V>接口
