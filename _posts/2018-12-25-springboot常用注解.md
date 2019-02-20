---
layout: post
title:  "springBoot常见注解"
date:   2018-12-24 01:06:05
categories: 微服务
tags: springboot
excerpt: springboot注解
---


* content
{:toc}



- @SpringBootApplication


```

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
		@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
    ......
}


```

该注解是启动Spring Boot应用的注解，实际上它是一个复合Annotation。

- @SpringBootConfigration


```

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
 

```
该注解继承自@Configuration，两者的功能也一致，就是标注当前类为一个配置类。会将当前类声明的一个或多个以@Bean注解标记的方法实例纳入到Spring容器中，并且实例名就是方法名。

其父注解@Configuration就是JavaConfig形式的Spring Ioc容器配置类使用的那个@Configuration。Spring Boot启动时，需要加载的IoC容器的配置即由此定义。



- @ServletComponentScan


```
在 SpringBootApplication 上使用@ServletComponentScan 注解后，Servlet、Filter、Listener 可以直接通过 @WebServlet、@WebFilter、@WebListener 注解自动注册，无需其他代码。
```

- @EnableAutoConfiguration

@EnableAutoConfiguration的理念就是借助@Import的支持，收集和注册特定场景相关的Bean的定义。比如：

1. @EnableScheduling是通过@Import将Spring调度框架相关的Bean定义加载到IoC容器。
2. @EnableCaching是通过@Import将Spring缓存框架（如Guava等）相关的Bean定义加载到IoC容器。

而@EnableAutoConfiguration也是借助@Import的帮助，将所有复合自动配置条件的Bean定义加载到IoC容器。仅此而已。


```
@SuppressWarnings("deprecation")
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(EnableAutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
    ......
}

```
其中最关键的要属@Import(EnableAutoConfigurationImportSelector.class)，借助EnableAutoConfigurationImportSelector这个类，@EnableAutoConfiguration可以帮助Spring Boot应用将符合条件的@Configuration配置都加载到当前SpringBoot创建并使用的IoC容器。

具体的加载方式是通过SpringFactoriesLoader从指定的配置文件：META-INF/spring.factories加载配置。

- @ComponentScan

@ComponentScan的功能其实就是自动扫描并加载符合条件的组件或Bean定义，并最终将这些Bean定义加载到容器中。其作用是：

1. 自动扫描路径下边带有@Controller，@Service，@Repository，@Component注解加入Spring IoC容器。
2. 通过includeFilters加入扫描路径下没有以上注解的类加入spring容器。
3. 通过excludeFilters过滤出不用加入spring容器的类。


- @Conditional

在Spring框架中，我们可使用@Conditional配合@Configuration或者@Bean等注解来干预一个配置或者Bean定义是否生效，其最终实现效果或者语义如下：


```
if(符合@Conditional规定的条件) {
    加载当前配置或者注册当前Bean定义；
}
```
要实现基于条件的配置，我们只需要通过@Conditional指定自己的Condition实现类就可以了：

```
@Conditional({TheCondition1.class, TheCondition2.class, ...})
```
@Conditional可以作为父注解来标注其他注解实现类，从而构建各种各样的复合注解。SpringBoot实现了一批这样的注解用于简化开发：


```
1. @ConditionalOnClass
2. @ConditionalOnBean
3. @ConditionalOnMissingClass
4. @ConditionalOnMissingBean
5. @ConditionalOnProperty
```

- @AutoConfigureBefore／@AutoConfgureAfter

```
在实现自动配置的过程中，除了基于条件配置，我们还需要提供配置或组件加载的顺序，从而让这些配置或者组件之间的依赖分析和组装可以顺利完成。
```

- @Value

1. 普通字符串：@Value("This is a normal string") private String normalString;
2. 操作系统属性：@Value("#{systemProperties['os.name']}") private String osName；
3. 表达式结果：@Value("#{ T(java.lang.Math).random() * 20.0 }") private double theNum;
4. 文件资源：@Value("classpath:WEB-INF/spring.properties") private Resource resourceFile;
5. 其他Bean属性：@Value("#{otherBean.thatProperty}") private String propertyFromOtherBean;

```
其中在application.properties文件中配置的变量，Spring Boot在启动时自动加载。

```

- @PropertySource

```
@PropertySource用于读取指定目录下的文件，并加载。如：
@PropertySource(value = {"classpath:system-config.properties"},encoding="utf-8")

```
- @Autowired

@Autowired是Spring 3.0引入的一个注解，可以标注在类的属性上，这样Spring容器就会以byType的方式来注入对应的Bean。

- @Resource

@Resource默认按照ByName自动注入，由J2EE提供，需要导入包javax.annotation.Resource。

@Resource有两个重要的属性：name和type，而Spring将@Resource注解的name属性解析为Bean的名字，而type属性则解析为Bean的类型。所以:

1. 如果使用name属性，则使用byName的自动注入策略.
2. 而使用type属性时则使用byType自动注入策略。
3. 如果既不制定name也不制定type属性，这时将通过反射机制使用byName自动注入策略。

- @Component

@Component用于将类实例化并注入IoC中。其他注解@Controller，@Service，@Respository的用法跟@Component差不多，比@Component带有更多的语义，他们分别对应了控制层，业务层和持久层的类。

- @RequestMapping

@RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。 该注解有六个属性：

1. params：指定request中必须包含某些参数值是，才让该方法处理。
2. headers：指定request中必须包含某些指定的header值，才能让该方法处理请求。
3. value：指定请求的实际地址，指定的地址可以是URI Template 模式
4. method：指定请求的method类型， GET、POST、PUT、DELETE等
5. consumes：指定处理请求的提交内容类型（Content-Type），如application/json,text/html;
6. produces：指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回

- @RequestParam


```
该注解用在方法的参数前面。其作用是获取request.getParameter(方法参数)的值，并将取得的值付给该参数。
```

- @PathVariable

路径变量。参数与大括号里的名字一样要相同。如：


```
@RequestMapping("user/get/user/{address}")
public String getByUserAddress(@PathVariable String address){
　　//do something;
}
```

- @ResponseBody


```
使用@ResponseBody表示该方法的返回结果直接写入HTTP response body中。
```

- @RequestBody


```
@RequestBody用于读取Request请求的body部分数据，使用系统默认配置的HttpMessageConverter进行解析，然后把相应的数据绑定到要返回的对象上；再把HttpMessageConverter返回的对象数据绑定到 controller中方法的参数上。
```
- @PostConstruct


```
@Documented
@Retention(value=RUNTIME)
@Target(value=METHOD)
public @interface PostConstruct
```
PostConstruct 注释用于在依赖关系注入完成之后需要执行的方法上，以执行任何初始化。此方法必须在将类放入服务之前调用。支持依赖关系注入的所有类都必须支持此注释。即使类没有请求注入任何资源，用 PostConstruct 注释的方法也必须被调用。只有一个方法可以用此注释进行注释。应用 PostConstruct 注释的方法必须遵守以下所有标准：该方法不得有任何参数，除非是在 EJB 拦截器 (interceptor) 的情况下，根据 EJB 规范的定义，在这种情况下它将带有一个 InvocationContext 对象 ；该方法的返回类型必须为 void；该方法不得抛出已检查异常；应用 PostConstruct 的方法可以是 public、protected、package private 或 private；除了应用程序客户端之外，该方法不能是 static；该方法可以是 final；如果该方法抛出未检查异常，那么不得将类放入服务中，除非是能够处理异常并可从中恢复的 EJB。



- @PreDestroy


```
 被@PreDestroy修饰的方法会在服务器卸载Servlet的时候运行，并且只会被服务器调用一次，类似于Servlet的destroy()方法。被@PreDestroy修饰的方法会在destroy()方法之后运行，在Servlet被彻底卸载之前。
```

- @Import：用来导入其他配置类。
- @ImportResource：用来加载xml配置文件。

- @ControllerAdvice

```
包含@Component。可以被扫描到。统一处理异常。

```
- @ExceptionHandler（Exception.class）


```
用在方法上面表示遇到这个异常就执行以下方法。
```

- @SerializedName

```
指定序列化的key名和属性对应值
```