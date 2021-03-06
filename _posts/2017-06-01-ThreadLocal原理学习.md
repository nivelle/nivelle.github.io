---
layout: post
title:  "ThreadLocal原理学习"
date:   2017-07-01 00:06:05
categories: java
tags: 多线程
excerpt: ThreadLocal原理学习
---


* content
{:toc}


###  ThreadLocal

背景：ThreadLocal是java语言提供的用于支持线程局部变量的标准实现类。它通过避免对象的共享，达到线程安全的目的。

虽然是通过ThreadLocal来设置特定于各个线程的数据资源，但ThreadLocal自身不会保存这些特定的数据资源。因为数据资源特定于线程，自然是由每个线程自己来管理了。每个Thread类都有一个ThreadLocal.ThreadLocalMap的实例变量，它就是保持那些通过ThreadLocal设置给这个线程的数据资源的地方。当通过Threadlocal的Set(data)方法设置数据的时候，ThreadLocal会首先获取当前线程的引用，然后通过该引用获取当前线程持有的threadLocals,最后，以当前ThreadLocal作为Key,将要设置的数据设置到当前线程，如下：

```
Thread thread = Thread.currentThrad();

ThreadLocalMap threadlocalmap = thread.threadLocals;
...
threadlocamap.set(this,obj);

```
余下的get()之类的方法：首先取得当前线程，然后根据每个方法的语义，也可以通过这个窗口获取绑定数据资源，当然，更可以解除之前绑定到当前线程的数据资源。在整个线程的生命周期内，我们都可以通过ThreadLocal这个窗口与当前线程大交道。

![image](http://7xpuj1.com1.z0.glb.clouddn.com/Threadlocal.png)

####  ThreadLocal的应用场景

 - 横向看，我们更注重于ThreadLocal横跨多个线程的能力，这是ThreadLocal最初的目的所在。为了以更加简单地方式来管理应用程序的线程安全，ThreadLocal干脆将没有必要共享的对象不共享，直接为每个线程分配一份各自特定的数据资源。

- 纵向看，我们则着眼于ThreadLocal在单一线程内可以发挥的能力，通过ThradLocal设置的特定于各个线程的数据资源，可以随着所在的线程执行流程“随波逐流”


---

- 管理应用程序实现中的线程安全：对于某些有状态或者非线程安全的对象，我们可以再多线程程序中为每个线程都分配相应的副本，而不是让多个线程共享该类型的某个对象，从而避免了需要协调多个线程对这些对象进行访问的“危险”工作。

在JDBC中，Connection对象属于有状态并且非线程安全的类。为了保证多个线程使用Connection进行数据访问过程中的安全，我们通过ThreadLocal为每个线程分配了一个他们各自持有的Connection,从而避免了对单一Connection资源的竞争。毕竟，在JDBC中是一个Connection对应一个事务。如果所有线程都公用一个Connection，事务管理就失控了。

- 实现当前程序执行流程内的数据传递。通过ThreadLocal来保存某种全局变量，在线程内执行流程的某个时点设置该全局变量，然后在合适的位置获取其值以做某些判断。只要愿意，任何合适的数据都可以通过ThreadLocal在单一线程内传递数据这一功能进行传递。
-  某些情况下的性能优化。本质上就是以空间换时间，避免不必要共享的对象共享。
-  per-thread Singleton.当某项资源的初始化代价较大，并且在整个执行流程中还会多次访问它的时候，为了避免在访问时每次都需要去初始化该项资源，我们可以在第一将该资源初始化完成之后，直接通过ThreadLocal将其绑定到当前线程，之后，所有对该资源的访问都从当前线程直接获取。

### 使用ThreadLocal管理多数据源切换的条件

思路：通过ThreadLocal保存每个数据源对应的标志（该标志我们以枚举形式给出），AbstractRountingDataSource在通过determineCurrentLookuoKey()获取对应数据源的键值得时候，直接从ThreadLocal获取当前线程所持有的数据源标志然后返回。而至于说什么情况下使用哪个具体的数据源问题，则是由应用层序的需求来决定，只要在必要的地方，将所要使用的具体数据源对应标志通过ThreadLocal绑定到当前线程即可。

流程如下：

(1) 假设有三个数据源可用，MAIN,INFO和DBLINK三个数据源可用。第一步给出个枚举类：
```
public enum DataSources{
    MAIN,INFO,DBLINK;
}

```
(2)定义所使用的ThreadLocal的DataSourceTypeManager类定义

```
public class DataSourceTypeManager{
    private static final ThreadLocal<DataSources> dsTypes = new ThreadLocal<DataSources>(){
        @Override
        protected DataSources initialValue(){
            return DataSources.MAIN;
        }
    }
    
    public static DataSources get(){
        return dsTypes.get();
    }
    
    public static void  set(DataSources DataSourceType){
        return dsTypes.set(DataSourceType);
    }
}


```

(3) 有了标志枚举类和相应的ThreadLocal定义之后，就可以实现我们的AbstractRountingDataSource了：

```
public class ThreadLocalVariableRountingDataSource extends AbstractRountingDataSource{
    @Override
    protected Object determineCurrentLookupKey(){
        return DataSourceTypeManager.get();
    }
}


```
(4)将相关配置注册到IoC容器。

```
<bean id="dataSource" class ="...ThradLcoalVariableRountingDataSource">
 <property name ="defaultTargetDataSource" ref=""/>
 <property name="targetDataSources"
   <map key-type="..DataSources">
     <entry key="MAIN" value-ref="mainDataSource"/>
     <entry key="INFO" value-ref=infoDataSource"/>
     <entry key="DBLINK" value-ref="dblinkDataSource"/>
   </map>

 </property>
</bean>

```
(5)我们可以使用如下代码是的数据源的切换生效：

```
DataSourceTypeNabager.set(DataSources.INFO);

或者

DataSourceTypeNabager.set(DataSources.DBLINK);

```

此后通过ThreadLocalVariableRountingDataSource所进行的数据访问，则会使用我们所设定的具体数据源。

