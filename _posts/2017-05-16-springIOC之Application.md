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

applocationContext继承了ResourcePatternResolver，当然就间接实现了ResourceLoader接口。





































  


#####  国际化信息支持


#####  容器内部事件发布

