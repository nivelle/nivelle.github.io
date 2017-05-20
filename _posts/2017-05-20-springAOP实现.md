---
layout: post
title:  "spring学习之AOP实现"
date:   2017-04-23 00:06:05
categories: 技术
tags: spring
excerpt: spring之AOP实现
author: nivelle
---


* content
{:toc}

---

###  @AspectJ形式的Spring AOP 初览

- 支持AspectJ5发布的@Aspectj形式的AOP实现方式。现在我们可以直接使用POJO来定义Aspect以及相关的Advice，并使用一套标准的注解标注这些POJO。 Spring AOP会根据注解信息查找相关的Aspect定义，并将其声明的横切逻辑织入当前系统。

- 简化了XML配置方式。使用新的XSD得配置方式，我们可以使用AOP独有的命名空间，并且注册和使用POJO形式实现AOP概念实体。因为引入了AspectJ的Pointcut描述语言,也可以在配置方式使用AspectJ形式的Pointcut表达式。

```
@Aspect
public class PerformanceTraceAspect
{
    @Pointcut("execution(public void *.method1())||execution(public void *.method2())")
    public void pointcutName(){}
    
    @Around("pointcutName()")
    public Object performanceTrace(ProceedingJoinPoint joinpoint)
    {
        stopWatch watch = new StopWatch();
        try
        {
            watch.start()
            return joinpoint.proceed();
        }finall
        {
            watch.stop();
        }
    }
}


```
Aspect 中可以定义多个Pointcut以及多个Advice,所以除了要使用@Aspect标注Aspect类之外，还需要通过名为@Pointcut的注解指定Pointcut定义，通过@Around等注解来指定那些方法定义类相应的Advice逻辑。

```
public class Foo{
    public void method1()
    {};
    public void method2()
    {};
}

```
@AspectJ 形式的AOP织入方式之编程方式织入

通过AspectJProxyFactory,我们就可以实现Aspect定义到目标对象的织入：

```
AspectJProxyFactory weaver = new AspectJProxyFactory();
weaver.setProxyTargetClass(true);
weaver.setTarget(new Foo());
weaver.addAspect(PerformanceTraceAspect.class);
Object proxy = weaver.getProxy();
((Foo)proxy).method1();
((Foo)proxy).method2();

```
@AspectJ 形式的AOP织入方式之自动代理织入

Sping AOP专门提供了一个AutoProxyCreator实现类进行自动代理，即org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator。它的父类是AspectJAwareAdvisorAutoProxyCreator。

```
<bean class="org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator">
   <property name="proxyTargetClass" value="true"></property>
</bean>

<bean id="PerformanceTraceAspect" class="...">
</bean>
<bean id="target" class="..Foo">
</bean>

```
现在，org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator会自动收集IOC容器中注册的Aspect,并应用到Pointcut定义的各个目标对象上，如果我们通过容器取得现在的target对象的话，会发现它已经是被代理过的了

```
ApplicationContext ctx = new ClassPathXmlApplicationContext("...");
Object proxy = ctx.getBean("target");
((Foo)proxy).method1();
((Foo)proxy).method2();
```
org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator 注册到容器的方式是基于DTD的配置方式，在Spring1x以及2x版本中都可以使用。若使用2x版本，且使用XSD的配置方式，还可以有一种简洁的配置方式：

```
<aop:aspectj-autoproxy proxy-target-class="true">
</aop:aspectj-autoproxy>

<bean id="PerformanceTraceAspect" class="...">
</bean>
<bean id="target" class="..Foo">
</bean>
```
 
- @AspectJ形式的Pointcut 申明方式

@AspectJ形式的Pointcut申明依附在@Aspect所标注的Aspect定义类之内，通过使用org.aspectj.lang.annotation.Pointcut这个注解，指定AspectJ形式的Pointcut表达式之后，将这个指定了相应表达式标注到Aspect定义类
的某个方法上即可。

```
@Aspect
public class YourAspect
{
@Ponucut("aspectj style pointcut expression")//pointcut_expression
public void ponitcutMethod1(){}//poncut_signature

@Ponucut("aspectj style pointcut expression")
public void ponitcutMethod2(){}
}


```
@AspectJ形式的Pointcut申明组成：

- Pointcut Expression: Pointcut Expression的载体是@Pointcut,方法级别注解，所以Poictcut Expression 不能脱离某个方法单独申明。Pointcut Expression 附着于上的方法称为该Pointcut Expression 的Pointcut Signature.Pointcut Expression 是真正规定Pointcut 匹配规则的地方，可以通过@Pointcut直接指定AspectJ形式的Pointcut表达式。Pointcut表达式：
  -  Pointcut标志符。将以什么样的行为来匹配表达式
     - execution(modifiers-pattern?ret-type-pattern declaring-type-pattern?name-pattern(param-pattern) throws-pattern?)其中方法的返回类型、方法名以及参数部分的匹配模式是必须指定的，其余部分可以省略。
      #####　　两种通配符
      
      (1) *  可以用于任何部分的匹配模式中，可以匹配*相邻*的多个字符，即一个word.例如：execution(* *(*))
      
      (2) .. 通配符可以在两个位置使用，一个是在declaring-type-pattern规定的位置，一个是在方法参数匹配模式的位置。如果用于declaring-type-pattern 规定的位置则可以指定多个层次的类型声明：如下：
      
      ```
      execution (void cn.spring21.*.doSomething(*))；//只能指定到cn.spring21这一层下的所有类型

      execution (void cn.spring21..*.doSomething(*));//可以匹配cn.spring21包下的所有类型，以及cd.sprig21下层包下申明的所有类型。

      如果..用于方法参数列表匹配位置，则表示该方法可以有0到多个参数，参数类型不限。如下“
      execution(void *.doSomething(..)))但是，如果在这里使用*,则只能匹配一个参数。

      常见组合：
      - execution(void doSomething(String,*))
      
      //匹配两个参数的doSomething方法，第一个参数为String类型，第二个参数类型不变
      
      - execution(void doSomething(..,sting)
      
      //匹配拥有多个参数的doSomething方法，之前参数类型不限，但最后一个参数类型必须为String 
      
      - execution(void doSomething(*,String,..))
      
      //匹配拥有多个参数的doSomething方法，第一个参数类型不限，第二个必须为String,其他剩余参数数量类型均不限。
      
       ```
      (3) within 只接受类型声明，它将会匹配指定类型下所有的Joinpoint。我们为within指定某个类后，它将匹配指定 类所申明的所有方法执行。
     
      within(cn.spring21.aop.target.*);//匹配cn.spring21.aop.target包下所有类型内部的方法级别的Joinpoint

      within(cn.spring21.aop..*);//匹配cn.spring21.aop以及子包下所有类型的内部方法级别的Joinpoint
      
      (4) this和target： this 指代调用方法一方所在的对象，target指代被调用方法所在对象。

  -  表达式匹配模式。可以指定具体的匹配模式

- Pointcut Singature 。 在这里具体化为一个方法定义，它是Pointcut Expression 的载体。Pointcut signature 所在的方法定义，除了返回类型必须是void之外，没有其他限制。public 类型的Pointcut Signature 可以在其他Aspect定义中引用，private 则只能在当前Aspect中引用。可以作为相应Pointcut Expression的标识符，在Pointcut Expression的定义中取代重复的Pointcut表达式。

```
@Aspect
public class YourAspect
{
@Pointcut("execution{void method1()}")
public void method1Execution(){}

@Pointcut("method1Execution()")
private void stillMethod1Execution(){}
}


```
stillMethod1Execution()的Pointcut Expression,通过第一个Ponintcut定义的Pointcut Signature即method1Execution(),引用了第一个Pointcut定义，所以这两个Pointcut定义的规则是一样的。
