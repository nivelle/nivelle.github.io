---
layout: post
title:  "spring之xml"
date:   2017-05-10 00:06:05
categories: spring
tags: spring
excerpt: spring之xml
author: nivelle
---

* content
{:toc}

----

## \<beans>和\<bean>

#### \<beans> 是xml文件中最顶层的元素，它下面可以包含0或者1个\<description>和多个\<bean>、\<import>、\<alias>

做为顶层元素，它有一些具有全局范围的属性值：

 1. default-lazy-init:默认为false.标志是否对<bean>进行延迟初始化

 2. default-autowire:如果使用自动绑定，标志全体bean 使用哪一种默认绑定方式。可取值为：byName,byType,constructor以及autodetect

 3. default-dependency-check.可取值为none,objects,simple,all.默认为none 

 4. default-init-method.管辖所有bean均用到的初始化方法。

 5. default-destory-method.管辖bean均用到的对象销毁方法。


- \<description>、\<import>、\<alias>

 \<description>：指定一些描述信息。
 
 \<import> ：以\<import resource="other.xml"> 的方式加载main.xml所依赖的配置文件加载进来。

 \<alias> ：可以为某个bean起一个别名，目的是为了减少输入。

- \<bean> 

 每个业务对象作为个体，在spring中的xml配置文件中是与\<bean>一一对应的。
 
 ```
   <bean id ="djNEwsListener" class="..impl.DowJonesListener"></bean>

 ```

 属性：

 1. id:是bean在容器中的标志。同时还可使用name属性来指定\<bean>的别名。
 比如
 
```
 <bean id ="djNEwsListener" name ="/news/djNewsListener,dowJonesNewsLisener" class="..impl.DowJonesListener"></bean>
```

 2. class:每个注册到容器的对象都要通过class 指定其类型。

####  依赖关系的确认

#####  构造方法注入xml之道


```
<bean id="djNewsProvider" class="..FXNewsProvider">
     <constructor-arg ref ="djNewsListener"/>
     <constructor-arg ref="djNewsPersister"/>
</bean>

```
有时，无法确认配置项与构造方法参数列表的一一对应关系时，需要type或者index出马。

#####  type

```
public class MockBusinessObejct {
   
    private String dependency1;
    
    private int dependency2;
    
    public MockBusinessObejct(String dependency1) {       
        this.dependency1 = dependency1;
    }
    
    public MockBusinessObejct(int dependency2) {       
        this.dependency2 = dependency2;
    }

    @Override
    public String toString() {
        return "MockBusinessObejct [dependency1=" + dependency1 + ", dependency2=" + dependency2 + "]";
    }
    
}

```
此时配置文件：

```
<bean id="MockBO" class="..MockBusinessObejct">
  <constructor-arg>
    <value>11111\</value>
  </constructor-arg>
</bean>

```
如果从BeanFactory取得对象， 并调用toString查看的话，我们发现他其实调用的是第一个构造方法。此时可以通过制定参数类型来让其调用第二个方法.

```
<bean id="MockBO" class="..MockBusinessObejct">
  <constructor-arg type="int">
     <value>11111</value>
  </constructor-arg>
</bean>

```

#####  index

当某个业务对象构造方法传入多个类型相同的参数，可以通过index手动指定配置文件参数顺序和实体类参数一一对应，最好的方式是配置方式和实体类里参数顺序一致。

##### setter方法注入之道

与构造方法注入使用<constructor-arg>注入配置相对应，spring 为setter方法注入提供了\<property>元素。

\<property>有一个name属性，用来指定该\<property>将会注入的对象所对应的实例变量名称。之后通过value或者ref属性或者内嵌的其他元素来指定具体的依赖对象引用或者值。



**实际上**，可以同时使用两种方式元素实现依赖关系的确认。

```
<bean id="mockBO" class="..MockBusinessObject">
   <constructor-arg value="1111"/>
   <property name="dependency2" value="222"/>
</bean>

```

同时需要MockBusinessObejct 提供一个只有一个String 类型参数的构造方法，并且为dependency2提供相应的setter方法。'

```
 public MockBusinessObejct(String dependency1) 
    {       
        this.dependency1 = dependency1;
    }

 public void setDependency2(int dependency2) 
    {
        this.dependency2 = dependency2;
    }   

```

##### \<property>和\<constructor-arg>中可用配置

1. \<value>:可以通过value为主体对象注入简单的数据类型，不但可以指定String类型数据，而且可以指定其他java语言中的原始类型以及他们的包装器类型。

2. \<ref> :使用ref来引用容器中其他的对象实例，可以通过ref的local、parent和bean属性来指定引用的对象的beanName.

   - local:只能指定与当前位置对象在同一个配置文件的对象定义的名称

   - parent:只能指定位于当前容器的父容器中指定的对象引用

   - bean:通吃。
   其中\<ref>定义为<!ELEMENT ref EMPTY>表示下面无子元素了。

3. \<idref>:为当前对象注入所依赖的对象名称而不是引用。

4. 内部\<bean>:有时我们所依赖的对象只有当前一个对象引用，或者某个对象定义我们不想其他对象通过<ref>引用到它。

```
 <bean id="djNewsProvider" class="...FXNewsProvider">
   <constructor-arg index=0>
       <bean class="...impl.DowJoneNewsListener">\</bean>
   </constructor-arg>
 
   <constructor-arg index=1>
       <ref bean="djNewsPersister" 
   </constructor-arg>
 
```
这样，该对象实例就只有当前的djNewsProvider可以使用，其他对象无法取得该对象的引用。

5.\<list> 对应注入对象类型为java.util.List及其自雷或者数组类型的依赖对象。

6.\<set>  对应注入对象类型为java.util.Set,对set来说元素的顺序本来就是无关紧要的。

7.\<map>  映射可以通过制定的键来获取相应的值。对应对象类型是java.util.map.

```
public class MockDemoObjec
{
   private Map mapping;
}

配置类似于

<property name ="mapping">
    <map>
        <entry key="strValueKey">        
            <value>somthing</value>
        </entry>

        <entry>        
            <key>objectKey</key>
            <ref bean="somthingObject"/>
        </entry>

        <entry key-ref="lstKey">
       <list>
            。。。。
       </list>
       </entry>
   </map>
</property>

```

8.是简化了的\<map> 对应的java类型是java.util.Properties的对象依赖。而且键的类型只能是String类型的。

#####  depends-on

大部分用到的场景都是拥有静态代码块初始化代码或者数据驱动注册之类的场景，实现在初始化时首先初始化其依赖的实例。


####   autowire

除了可以通过明确指定bean之间的依赖关系，还可以根据某些特点将相互依赖的bean自动绑定的功能。免去手动指定。

-  \ no

默认自动绑定模式，完全依赖手工明确配置各个bean之间的依赖关系。

- byName

按照类中申明的实例变量名，与xml配置文件中申明的bean定义的beanName的值进行匹配，相匹配的bean定义将被自动绑定到当前实例变量上。

```
public class Foo {
  
  private  Bar emphasisAttribute;

}

class Bar{
  
}

那么应该使用如下代码所示实现自动绑定：

<bean id ="fooBean"  calss="...Foo" autoWire="byName">
</bean>

<bean id = "emphasisAttribute" class="...Bar">
</bean>


```
注意：第二个定义的id为emphasisAttribute 与Foo类中的实例变量名称相同。


- byType

根据类型分析其对应的依赖实例，然后实现自动绑定。只存在一个符合条件的依赖对象时才发挥最大作用。


- constructor 

以上两种是基于property绑定的，而constructor 是针对构造方法参数类型而进行的自动绑定，它同样是byType类型的绑定模式。不过匹配的是构造方法中的参数类型，而不是实例属性的类型。

```
public class Foo {
  
  private  Bar bar;
  
  public Foo(Bar arg){
    this.bar =arg;
  }

}

class Bar{
  
}

相应配置为：

<bean id ="foo" class="..Foo" autowire="constructor"/>

<bean id="bar" class= "..bar"/>

```

- \autodetect

是byType和 constructor 的结合体，若有无参构造方法，容器会优先考虑byType自动绑定模式，否则会使用constructor模式。如果通过constructor绑定后还有其他属性没有绑定，容器会用bytype对剩余属性进行绑定。


#####  dependency-check

主要和自动绑定一起使用，用以检查每个对象某种类型的所有依赖是否全部已经完全注入完成。

  -  none :不做依赖检查。

  -  simple ；对简单属性类型以及相关的collection进行依赖检查，对象引用类型的依赖除外。

  -  object：只对对象引用类型依赖检查。
  
  -  all: 全部检查。



#####  lazy-init

主要针对 applicationContext 容器的bean初始化进行控制。applicationContext启动时，会初始化索引的"singleto的bean定义"。

```
<bean id="lazy-init-bean" class"..." lazy-init="true">

```
 
 ----


##  xml 中的继承

  通过\<bean>的abstract属性为true,申明为不需要实例的模板类。通过parent属性指定其父类的属性。

  其中applicationtext启动的时候不会实例化abstract的bean.


##  bean的scope

用来申明容器中的对象所应该处的限定场景或者说对象的存活时间，容器在对象进入相应的scope之前，生成装配这些对象，对象不在scope限定之后，容器通常会销毁这些对象。

- singleton

标记为scope的对象定义，在Spring 的IOC 容器中只存在一个实例，所有对该对象的引用将共享这个实例。它从容器启动第一次请求创建后，一直存活到容器退出而结束。

![image](http://7xpuj1.com1.z0.glb.clouddn.com/singleton.png)

- prototype

容器在接到给类型对象的请求的时候，会每次重新生成一个对象实例给请求方法，容器只负责创建并返回给请求方，交给请求方之后，该对象的生命周期的管理工作，包括销毁都必须由客户端自行处理了。

主要面对请求方不能共享使用的对象类型，应将其bean定义为scope=prototype

![image](http://7xpuj1.com1.z0.glb.clouddn.com/prototype.png)

- request

    只适用于web应用程序，通常是与XmlWebApplicationContext共同使用。

    spring容器及XmlWebApplicationContext 会为每个http请求创建一个全新的RequestProcessor对象供当前请求使用，当请求结束后，该对象实例的生命周期即结束。

- session 

    只适用于web应用程序，通常是与XmlWebApplicationContext共同使用。

    除了比reques拥有更长的存活时间，其他方面是一样的。


- global session 

    如果在基于servlet的web应用中使用这个类型的scope，将会视其为普通session对应的scope.


---
文章内容出自：《spring揭秘》--王福强
    
