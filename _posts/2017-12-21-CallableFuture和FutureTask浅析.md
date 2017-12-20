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

Future<V>接口是用来获取异步计算结果的，说白了就是对具体的Runnable或者Callable对象任务执行的结果进行获取(get()),取消(cancel()),判断是否完成等操作。我们看看Future接口的源码：

```
public interface Future<V> {  
    boolean cancel(boolean mayInterruptIfRunning);  
    boolean isCancelled();  
    boolean isDone();  
    V get() throws InterruptedException, ExecutionException;  
    V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;  
}  


```

方法解析:

V get() ：获取异步执行的结果，如果没有结果可用，此方法会阻塞直到异步计算完成。

V get(Long timeout , TimeUnit unit) ：获取异步执行结果，如果没有结果可用，此方法会阻塞，但是会有时间限制，如果阻塞时间超过设定的timeout时间，该方法将抛出异常。

boolean isDone() ：如果任务执行结束，无论是正常结束或是中途取消还是发生异常，都返回true。

boolean isCanceller() ：如果任务完成前被取消，则返回true。

boolean cancel(boolean mayInterruptRunning) ：如果任务还没开始，执行cancel(...)方法将返回false；如果任务已经启动，执行cancel(true)方法将以中断执行此任务线程的方式来试图停止任务，如果停止成功，返回true；当任务已经启动，执行cancel(false)方法将不会对正在执行的任务线程产生影响(让线程正常执行到完成)，此时返回false；当任务已经完成，执行cancel(...)方法将返回false。mayInterruptRunning参数表示是否中断执行中的线程。

通过方法分析我们也知道实际上Future提供了3种功能：（1）能够中断执行中的任务（2）判断任务是否执行完成（3）获取任务执行完成后额结果。

**Future只是一个接口，我们无法直接创建对象**

### FutureTask类

我们先来看看FutureTask的实现:

```
public class FutureTask<V> implements RunnableFuture<V> {  


```

FutureTask类实现了RunnableFuture接口，我们看一下RunnableFuture接口的实现：

```

public interface RunnableFuture<V> extends Runnable, Future<V> {  
    void run();  
}  

```

FutureTask除了实现了Future接口外还实现了Runnable接口，因此FutureTask也可以直接提交给Executor执行。 当然也可以调用线程直接执行（FutureTask.run()）。接下来我们根据FutureTask.run()的执行时机来分析其所处的3种状态：

- 未启动，FutureTask.run()方法还没有被执行之前，FutureTask处于未启动状态，当创建一个FutureTask，而且没有执行FutureTask.run()方法前，这个FutureTask也处于未启动状态。
- 已启动，FutureTask.run()被执行的过程中，FutureTask处于已启动状态。
- 已完成，FutureTask.run()方法执行完正常结束，或者被取消或者抛出异常而结束，FutureTask都处于完成状态

**下面我们再来看看FutureTask的方法执行示意图**

![image](http://7xpuj1.com1.z0.glb.clouddn.com/futureTask.png)
