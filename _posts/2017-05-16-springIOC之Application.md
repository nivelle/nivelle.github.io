---

layout: post
title:  "springIOC之Application"
date:   2017-04-23 00:06:05
categories: 技术
tags: spring
excerpt: Application

---


* content
{:toc}

---

###  ApplicationContext

#####  统一资源加载策略
- Resource(资源抽象)和 ResourceLoader（资源加载策略）

  Resource接口可以根据资源的不同类型或者场合，给出对应的实现。Spring一些常用的实现类:
  
  - ByteArrayResource：
  
    将字节数组提供的数据作为资源封装，通过InputStrem访问资源，则会根据字节数组的数据，构造相应的ByteArrayInputStream并返回。
  - ClassPathResource: 
    
    从java应用程序的ClassPath中加载具体资源并进行封装，可以使用指定的类加载器（classLoader）或者给定的类进行资源的加载。 

  - FileSystemResource:
    
    对java.io.File类型的封装，可以以文件或者url的形式对该类型资源进行访问。

  - UrlResource:
  
    通过java.net.URL进行的具体资源查找定位实现类，内部委派URL进行具体的资源操作。

  - InputStreamResource
  
    经常以ByteArrayResource 代替。

```
资源接口定义：

public interface Resource extends inputStreamSource
{
     boolean exists();
     boolean isOpen();
     URL getURL() throws IOException;
     File getFile() throws IOException;
     Resource createRelative(String relativePath)throws IOException ;
     String getFilename();
     String getDescription();
}
```
要想实现自定义的Resource，可以直接继承org.springframework.core.io.AbstractResource抽象类足够。


- ResourceLoader 

org.springframewok.core..io.ResourceLoader接口是资源查找定位策略的统一抽象，具体查找策略由ResourceLoader实现类给出

```
public interface ResourceLoader
{
    String CLASSPATH_URL_PREFIX=ResourceUtils.CLASSPATH_URL_PREFIX;
    Resource getResource(String lication);
    ClassLoader getClassLoader();
}
```
可用的ResourceLoader

- DefaultResourceLoader
 
  (1)首先检查资源路径是否以classpath:前缀打头，如果是，则尝试构造ClassPathResource类型资源并返回
 
  (2)否则，(a)尝试通过URL,根据资源路径来定位资源，如果没有抛出MalformedURLException,有则会构造URLResource类型的资源返回(b)如果还是无法根据资源路径定位指定的资源，则委派getResourceByPath(String)方法来定位，DefautlResourceLoader的getResourceBypath(String) 方法默认实现是，构造ClassPathResource类型的资源并返回。

  (3)如果最终没有找到符合条件的相应资源，getResourceByPath(String)方法就会构造一个实际并不存在的资源返回，而指定有协议前缀的资源路径，则通过URL能够定位，所以返回的都是URLResource类型。

- FileSystemResourceLoader

  继承自DefaultResourceLoader,覆写了getResourceByPath(string)方法，使之从文件系统加载资源并以FileSystemResource类型返回。
  
- ResourcePatternResolver

可以根据根据指定的资源路径匹配模式，每次返回多个Resource实例。
```
public interface ResourcePatternResolver extends ResourceLoader
{
    string CLASSPATH_ALL_URL_PREFIX= "classpath*:";
    Resource[] getResources(String locationPattern) throws IOException;
}

```

ResourcePatternResolver 最常用的一个实现就是org.springframework.core.io.support.PathMatchingResourcePatternResolver.支持ResourceLoader级别的资源加载，支持基于ANT风格的路径匹配模式，支持ResourcePattern新增的classpath*:前缀等，基本上集所有技能于一身。

构造实例的时候可以指定一个ResourceLoader，如果不指定则默认为DefaultResourceLoader实例。PathMatchingResourcePatternResolver内部会将匹配后的资源路径为派给它的的ResourceLoader来查找和定位资源。

![image](http://7xpuj1.com1.z0.glb.clouddn.com/resource.png)


###  ApplicationContext 与ResourceLoader

applocationContext继承了ResourcePatternResolver，当然就间接实现了ResourceLoader接口。所以任何ApplicationContext实现都可以看做是一个ResourceLoader甚至ResourcePatternResolver。这就是ApplicationContext支持Spring内统一资源加载策略的真相。

通常，ApplicationContext直接或间接继承org.springframework.context.support.AbstractApplicationContext。AbstractApplicationContext 继承了DefaultResourceLoader,那么，它的getResource(String)当然就直接用DefaultResourceLoader的了。


AbstractApplicationContext 内部申明一个PathMatchingResourcePatternResolver，它可以接受一个resourceLoader,而AbstractApplicationContext本身又继承至DefaultResourceLoader，这样就把自身设置进入了。

![image](http://7xpuj1.com1.z0.glb.clouddn.com/AbsractApplicationContext.png)


---

###### Applocation的使用：

- 扮演ResourceLoader角色


```
ResourceLoader resourceLoader = new ClassPathXmlApplicationContext("配置文件路径");
Resource fileResource = resourceLoader.getResource("D:/spring21site/README");
```



- ResourceLoader 类型的注入

如果某个bean需要依赖于ResourceLoader来查找定位资源，我们可以为其注入容器中申明的某个具体的ResourceLoader实现。

或者通过实现ResourceLoaderAware和ApplicationContextAware接口可以通过依赖于SpringAPI 的来实现利用Application的容器作用。

```
public class FOoBar implements ApplicationContextAware
{
   private ResourceLoader resourceLoader;
   public void foo(String location)
   {
      system.out.println(getResourceLoader().getResource(location).getClass());
   }

   public ResourceLoader getResourceLoader()
   {
     return resourceLoader;
   }

   public void setApplicationContext(ApplicationContext ctx) throws BeansException
   {
     this.resourceLoader = ctx;
   }
}


```

这样，启动容器的时候，就会自动将当前的ApplicationContext容器本身注入到FooBar中，因为ApplicationContext类型容器可以自动识别Aware接口

- Resource 类型的注入

ApplicationContext容器可以正确识别Resource类型并转换后注入相关对象。而不用通过自定义propertyEditors注册到容器以供类型转换使用来识别Resource类型。


总结：appliicationContext 启动开始，会通过一个org.springframework.beans.support.ResourceEditorRegistrar来注册针对resource类型的PropertyEditor实现到容器中，这个PropertyEditor叫做org.springframework.core.io.ResourceEditor.

- 特定ApplicationContext的Resource加载行为

ResourceLoader中新增了一种新的资源路径协议----classpath: ,ResourcePatternResolver又增加了一种----classpath*:。

 (1)ClassPathXmlApplicationContext在实例化的时候，即使没有指明classpath:或者classpath*:等前缀，它会默认从classpath中加载bean定义的配置文件

 (2)FileSystemXmlAppliicationContext，如果不加classpath:或者calsspath*:,则会从文件系统加载配置文件。

----


#####  国际化信息支持

java中国际化信息处理，主要涉及两个类,即java.util.Locale和java.util.ResourceBundle




#####  容器内部事件发布

