---
layout: post
title:  "spring学习之AOP实现"
date:   2017-04-23 00:06:05
categories: 技术
tags: spring
excerpt: spring之AOP实现
---


* content
{:toc}

---

###  @AspectJ形式的Spring AOP

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

public class Foo{
    public void method1()
    {};
    public void method2()
    {};
}
```


#####  @AspectJ 形式的AOP织入方式

- 编程方式织入

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
