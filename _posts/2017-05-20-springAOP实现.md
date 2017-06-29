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
 
### @AspectJ形式的Pointcut 申明方式

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
     
##### 两种通配符
      
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
      
      (4) this和target： 在AspectJ中this 指代调用方法一方所在的对象，target指代被调用方法所在对象。而在Spring AOP中，this指代的是目标对象的代理对象，target指代的是目标对象，如果使用this(ObjectType)作为Pointcut定义，那么当目标对象的代理对象时ObjectType类型的时候，该Pontcut定义将匹配ObjectType类型中所有的Jointpoint.使用target(ObejctType)作为Pointcut定义，当目标对象是ObjectType类型的时候，该Pointcut定义将匹配ObjectType型目标对象内定义的所有Joinpoint。
      
      (5) @args 该标识符的作用是，帮助我们捕捉拥有指定参数类型、指定参数数量的方法级Joinpoint，而不管该方法在什么类型中被声明。

      (6) @within 如果使用@within指定了某种类型的注解，那么，只要对象标注了该类型的注解，使用@within标识符的Pointcut表达式将匹配该对象内部所有Joinpoint。

      (7) @target 如果目标对象拥有@target标识符所指定的注解类型，那么目标对象内部所有的Joinpoint将被匹配。对于Spring AOP来说，是目标对象中所有的方法级别Joinpoint将被匹配。

      (8) @args
      是一样@args标识符的Pointcut表达式将会尝试检查当前方法级的Joinpoint的方法参数类型。如果该次传入的参数类型拥有@args所指定的注解，当前Joinpoint将被匹配，否则将不会被匹配。@args会对系统中所有对象的每次方法执行的参数进行指定的注解的动态检查。只要参数类型标注有@args指定的注解类型，当前方法执行就将匹配，至于说参数类型是什么，它则不是十分关心。

      (9) @annotation 
       使用@annotation标识符的Pointcut表达式，将会尝试检查系统中所有对象所有方法级别Joinpoint。如果被检测的方法标注有@annotation标志符所指定的注解类型，那么当前方法所在的Joinpoint将被Pointcut表达式所匹配。

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

---

### @AspectJ 形式的Advice

@AspectJ形式的Advice定义，实际上就是使用@Aspect标注的Aspect定义类中的普通方法。只不过这些方法需要针对不同的Advice类型使用对应的注解进行标注。

可以用于标注对应Advice定义方法的注解包括：
 

 - @Before。用于标注Before Advice 定义所在的方法；

 - @AfterReturning。 用于标注After Returning Advice；

 - @AterThrowing。用于标注After Throwing Advice 定义所在的方法，也就是在Spring AOP 中称为ThrowsAdvice的那种Advice类型；

 - @After。用于标注After(finally)Advice定义所在的方法，也就是在Spring AOP中称为ThrowsAdvice的那种Advice类型；

 - @Around。用于标注Around Advice 定义所在的方法，也就是常说的的*拦截器*类型的Advice；

 - @DeclareParents。用于标注 Introduction 类型的Advice，但该注解对应标注对象的域（Field），而不是方法（Method）.

 ### 示例
 ```
 @Aspect
 public class MockAspect
 {
   @Pointcut("execution(*destroy(..))")
   public void destory(){}

   @Before("execution(public void *.methodName(String))")
   public void setUpResourceFolder()
   {
    ...
   }
   @After("destory()")
   public void cleanUpResourcesIfNecessary()
   {
    ...
   }

 }

 ```

 1. Before Advice

    以@AspectJ形式声明Before Advice,只需要在Aspct定义类中相应的方法，然后使用org.aspectj.lang.annotation.Before标注该方法即可。

 ```
 @Aspect
 public class ResourceSetupJAspect
 {
   private Resource resource;

   @Before("execution(boolean *.execute()))
   public void setupResourcesBefore() throws Throwable
   {
     if(!getResource().exists())
     {
       FileUtils.forceMkdir(getResource().getFile());
     }
   }
   public Resource getResource(){
     return resource;
   }

   public void setResource(Resource resource){
    this.resource = resource;
   }
 }

 ```   
 我们可能在Advice定义中访问Joinpoint处的方法参数，在1.x的Spring AOP 中，我们可以通过MethodBeforeAdvice的before方法传入的Object[]数组，访问相应的方法参数。如今可以有如下方法实现

 - 通过org.aspectj.lang.JoinPoint。

   在@AspectJ形式的Aspect中，定义Before Advice 的方法可以将第一个参数声明为org.aspectj.lang.JoinPoint类型，通过Joinpoint我们可以借助它的getArgs()方法，访问相应Joinpoint处方法的参数值。另外，我们还可以借助Radeon其他方法取得信息，比如，getThis()获得当前代理对象，getTarget()取得当前目标对象，getSignature()取得当前Jounpoint处的方法签名等。
   
   ```
   public void setupResourceBefore(JoinPoint joinpoint) throws Throwable{
     joinpoint.getArgs();
   }
   ```
 - 通过args标志符绑定。当args标识符接受的不是具体的对象类型，而是某个参数名称的时候，它会将这个参数名称对应的参数值绑定到对Advice方法的调用。
 
 我们可以在Before Advice对应的方法之上声明需要的参数，然后在Pointcut定义中使用args将这些声明的参数绑定到调用即可。

 ```
 @Before(value="execution(boolean *.execute(String,..))&&args(taskName)")
 public void setupResourcesBefore(String taskName) throws throwable
 {
   //访问'taskName'参数
   ...
 }

 ```

 2. After Throwing Advice

    ```
    @Aspect
    public class ExceptionBarrierAspect{

       private JavaMailSender mailSender;
       private String[] receiptions;

       @AfterThrowing(pointcut="execution(boolean *.execute(String,..))",thrown="e")
       public void afterThrowing(RuntimeException e)
       {
         final String exceptionMessage = ExceptionUtils.getFullStackTrace(e);
         getMailSender().send(new MimeMessagePreparator(){
            MimeMessagehelper helper = new MimemessageHelper(message);
            helper.setSubject("...");
            helper.setTo(getReceptions());
            helper.setText(exceptionMessage)
         })
       }
    }
    public JavaMailSender getMailSender(){
      return mailSender;
    }
    public String[] getReceptions(){
      return receiptions;
    }
    public void setReceiptions(String[] receptions){
      this.receptions = receptions;
    }


    ```
---

内容来自《Spring 揭秘》 作者：王福强
