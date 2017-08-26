---
layout: post
title:  "springMVC学习(一)"
date:   2017-06-10 00:06:05
categories: springMVC
tags: springMVC
excerpt: springMVC
author: nivelle
---

* content
{:toc}


![image](http://7xpuj1.com1.z0.glb.clouddn.com/springMVC%E6%A1%86%E6%9E%B6%E6%A8%A1%E5%9E%8B.png)

### SpringMVC请求处理过程:

- 整个过程始于客户端发出一个HTTP请求,web应用服务器接收到这个请求.如果匹配DispatcherServlet的请求映射路径(web.xml中指定),则web容器将该请求交给DispatcherServlet处理.
- DispatcherServlet接收到这个请求后,根据请求信息(URL,HTTP方法,请求报文头,请求参数,cookie等)以及handlerMapping的配置找到处理请求的处理器(handler).可将HandlerMapping看作路由控制器,将handler看作目标主机.spring MVC中没有定义一个handler接口,实际上,任何一个Object都可以成为请求处理器.
- DispatcherServlet根据HandlerMapping得到对应当前请求的Handler后,通过HandlerAdapter对Handler进行封装,再以统一的适配器接口调用Handler.HanddlerAdapter是SpringMVC的框架级接口,顾名思义,HandlerAdapter是一个适配器,它用统一的接口对各种Handler方法进行调用.
- 处理完成业务逻辑的处理后将返回一个ModelAndView给DispatcherServlet,ModelAndView包含了视图逻辑名和数据信息.
- ModelAndView中包含的是"逻辑视图名"而非真正的视图对象,DispathcerServlet借由ViewResoler完成逻辑视图名到真实视图对象的解析工作.
- 当得到真实的视图对象View后,DispatcherServlet就使用这个View对象对ModelAndView中的模型数据进行视图渲染.
- 最终客户端得到的响应消息可能是一个普通的HTML页面也可能是一个XML或者JSON串,甚至是一张图片或者一个PDF文档等不同的媒体形式.

### 配置DispatcherServlet

它负责接收HTTP请求并协调Spring MVC的各个组件完成请求处理工作.和任何Servlet一样,用户必须在web.xml中配置好DispatcherServlet.

1. 配置DispathverServlet,截获特定的URL请求

我们可以在web.xml中配置一个Servlet,并通过<servlet-mapping>指定其处理的URL.

```

   <!-- 配置Spring配置文件路径 -->
   
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>
            classpath*:spring/applicationContext.xml
        </param-value>
  </context-param>
  
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  

  <!-- Spring MVC 核心控制器 DispatcherServlet 配置 -->
  <servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath*:spring/spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>*.shtml</url-pattern>
  </servlet-mapping>



```

通过contextConfigLocation参数指定业务层Spring容器的配置文件(多个配置文件使用逗号分隔).ContextLoaderListener是一个ServletContextListener,它通过contextConfigLocation参数所指定的Spring配置文件启动业务层的Spring容器.

dispatcherServlet加载SpringMVC配置文件,启动web层的Spring容器.通过<Servlet-mapping>指定dispatcher处理所有以.shtml为后缀的http请求.


DispatcherServlet默认的规则可进行调整,常见配置参数,可通过<Servlet>的<init-param>指定.

- namespace:dispatcher对应的命名空间.默认的是<servlet-name>-servlet,用于构造Spring配置文件路径.在显示配置该属性后,配置文件对于的路径为 WEB-INF/<namespace>.xml,而非WEB-INF/<servlet-name>-servlet.xml.

- contextConfigLocation:如果dispatcherServlet上下文对于的Spring配置文件有多个,则可以根据该属性按照spring资源路径的方式指定.如:classpath:sample1.xml,classpath:sample2.xml. DispatcherServlet将使用类路径下的sample1.xml和sample2.xml这两个配置文件初始化WebApplicationContext.

- publicContext:布尔类型的属性,默认值为true.DispatcherServlet根据该属性决定是否将WebApplicationContext发布到ServletContext的属性列表,以便调用者可借由ServletContex找到WebApplicationContext的实例,对应的属性名为DispatcherServlet#getServletContextAttributeName()方法的返回值

- pubLicEvents:布尔类型的属性.当DispatherServlet处理完一个请求后,是否向容器发布一个ServletRequesHandleEvent事件,默认值为true.如果容器中没有任何事件监听器,则可以将该属性值设置为false,以便提高运行性能.

### dispatcherServlet的内部逻辑

```

protected void initStrategies(ApplicationContext context) {
        this.initMultipartResolver(context);
        this.initLocaleResolver(context);
        this.initThemeResolver(context);
        this.initHandlerMappings(context);
        this.initHandlerAdapters(context);
        this.initHandlerExceptionResolvers(context);
        this.initRequestToViewNameTranslator(context);
        this.initViewResolvers(context);
        this.initFlashMapManager(context);
    }

```

initStrategies()方法将在WebApplicationContext初始化后自动执行,此时Spring上下文中Bean已经初始化完毕.通过反射机制查找并装配Spring容器中用户显示自定义的组件bean,如果找不到则装配默认的组件实例.

MultipartResolver,LocaleResolver等,最多允许一个实例,而另一些则允许存在多个实例,如HandlerMapping,HandlerAdapter德国,同一个类型的组件如果存在多个,那么它们之间的优先级顺序通过org.springframework.core.Order接口,可通过order属性确定优先级顺序,值越小优先级越高.


![image](http://7xpuj1.com1.z0.glb.clouddn.com/dispatcherServlet.png)


#### 模拟处理流程

![image](http://7xpuj1.com1.z0.glb.clouddn.com/mvc%E7%BB%84%E4%BB%B6%E5%B7%A5%E4%BD%9C%E8%BF%87%E7%A8%8B.png)


- DispatcherServlet接收客户端的/user.html请求

- DispatcherServlet使用DefaultAnnotationHandlerMapping查找负责处理该请求的处理器

- DispatcherServlet将请求分发给名为/user.html的UserController处理器

- 处理完成业务后,返回ModelAndView对象,其中View的逻辑名为/user/success,而模型包含一个键为user的User对象

- DispatcherServlet调用InternalResourceViewResolver组件对ModelAndView中的逻辑视图名进行解析,得到真实的视图对象,/WEB-INF/view/user/createSuccess.jsp

- DispatcherServle使用/WEB-INF/view/user/createSuccess.jsp对模型中的user模型对象精细渲染

- 返回响应页面给客户端


### 注解驱动的控制器

在POJO类出定义标注Controller,再通过<context:component-scan/>扫描相应的类包,即可是POJO成为一个能处理HTTP请求的控制器.

@RequestMapping 根据一系列的映射规则,来完成映射功能.包括四个方面的选项

- 通过请求URL进行映射

在类出定义指定的URL相对于web应用的部署路径,而在方法定义处指定的URL相对于类定义处指定的URL.如果在类定义处未标注@RequestMapping,则仅在处理方法处标注@RequestMapping,此时,方法处指定的URL则相对于WEB应用的部署路径



这些URL可以是如下样式:


![image](http://7xpuj1.com1.z0.glb.clouddn.com/URLregix.png)

通过@PathVariable 可以将URL占位符参数绑定到控制器处理方法的入参中.

```
@Controller
@RequestMapping("/user")
public class UserController {
    @RequestMapping("/{userId}")
    public ModelAndView showDetail(@PathVariable("userId") String userId){
        ModelAndView mav = new ModelAndView();
        mav.setViewName("user/showDetail");
        mav.addObject("user",new Object());
        return  mav;
    }
}

```

- 通过请求参数,请求方法或请求头进行映射


![image](http://7xpuj1.com1.z0.glb.clouddn.com/http%E8%AF%B7%E6%B1%82%E6%8A%A5%E6%96%87.png)

1 是请求方法post和get是最常见的HTTP方法.spring提供了一个hiddenHttpMethodFilter,允许通过_method表单参数指定这些特殊的HTTP方法/服务器配置了HiddenHttpMethodFilter后,spring会根据_method参数指定的值模拟出相应的HTTP方法,这样就可以使用这些HTTP方法对处理方法进行

2 是请求对应的URL地址,他和报文头的HOST属性组成完整的请求URL

3 协议名称以及版本号

4 HTTP报文头,报文头包含入肝属性,格式"名称:值",服务器端根据此获取客户端的信息

5 报文体,它将一个页面表单中的组件通过param1=value1&param2=value2的键值对形式编码成一个格式化串,承载多个请求参数的数据.不但报文体可以传递请求参数,请求URL也可以通过类似于/chapter17/user,html?param1=value1&param2=value2的方式传递请求参数.

```
@Controller
@RequestMapping("/user")
public class UserController1 {
    @RequestMapping(path="/delete",method = RequestMethod.POST,params = "userId")
    public String test1(){
        return "user/test1";
    }
    
    @RequestMapping(path="/show",headers = "content-type=text/*")
    public String test2(@RequestParam("userId") String userId){
        return "user/test2";
    }
}


```
@RequestMapping 的value,method,params以及headers分别表示请求URL,请求方法,请求参数以及报文头的映射条件,它们之间是与的关系,联合使用多个条件项可嚷嚷请求映射更加精确化.

params和headers分别通过请求参数以及报文头属性进行映射,它们支持简单的映射表达式.下面params表达式为例进行说明,headers可以参照params来理解.

- "param1":表示请求须包含名为param1的请求参数

- "!param1":表示请求不能包含名为param1的请求参数

- "param1!=value1":表示请求包含名为param1的请求参数,但其值不能为value1.

- {"param1=value1,"param2"}:表示请求必须包含名为param1和param2的两个请求参数,且param1参数的值必须为value1


### 请求处理方法签名

SpringMVC通过分析处理方法的签名,将HTTP请求信息绑定到处理方法的相应入参中,然后在调用处理方法得到返回值,最后对返回值进行处理并返回响应.

将HTTP请求的信息绑定到相应的方法入参中,并根据方法返回值类型做出相应的后续处理.


![image](http://7xpuj1.com1.z0.glb.clouddn.com/%E5%87%A0%E7%A7%8D%E5%85%B8%E5%9E%8B%E7%9A%84%E5%A4%84%E7%90%86%E6%96%B9%E6%B3%95%E7%AD%BE%E5%90%8D.png)


#### 请求处理方法签名详细说明

1. 使用@RequestParam 绑定请求参数值

java类反射对象默认不记录方法入参的名称,因此需要在方法入参处使用@RequestParam注解指定其对应的请求参数.它包含三个参数

   - value:参数名

   - required:是否必需.

   - defaultValue:默认参数

2. 使用@ReuestHeader绑定请求报文头的属性值

与RequesParam类似,请求报文包含了若干个报文头属性,服务器可据此获知客户端的信息,通过@RequestHeader即可将报文头属性值绑定到处理方法的入参中.

```
@RequestMapping(path="/handle13")
public String handle13(@ReuestHeader("Accept-Encoding")String encoding,@RequestHeader("Keep-Alive")long keepAlive){
   return "";
}


```

3. 使用命令/表单对象绑定请求参数值

SpringMVC会按照请求参数名和命令/表单对象属性名匹配的方式,自动为该对象填充属性值.支持级联的属性名,如dept.deptId,dept.address.tel等


![image](http://7xpuj1.com1.z0.glb.clouddn.com/%E5%8F%82%E6%95%B0%E7%BB%91%E5%AE%9A.png)


4. 使用@CookieValue 绑定请求参数中Cookie值

使用@CookieValue可让处理方法入参绑定某个Cookie的值,它和@ReuestParam拥有3个一样的参数.

```
@RequestMapping(path="/handle13")
public String handle13(@RequestParam(value="userName",required=false) String userName,@RequestParam("age")int age){
   return "";
}

```

5. 使用Servlet API对象作为入参

使用Servlet API作为入参时,Spring MVC会自动将web层对应的Servlet对象传递给处理方法的入参,处理方法入参可以同时使用Servlet API类额入参和其他符合要求的入参,它们之间的位置顺序没有特殊要求.

** 值得注意的是,如果处理方法自行使用HttpServletResponse返回响应,则处理方法的返回值设置成void即可. **

6. 使用I/O对象作为入参

Servlet的ServletRequest拥有getInputStream()和getReader()方法,可以通过它们读取请求的信息.相应的,Servlet的ServletResponse拥有getOutputstream()和getWriter()方法,可以通过它们输出响应信息.

Spring MVC允许控制器的处理方法使用java.io.InputStream/java.io.Reader以及java.io.OutputStream/java.io.Writer作为方法的入参,Spring MVC 将获取ServletRequest的InputStream/Reader或ServletResponse的OutputStream/Writer,然后传递给控制器的处理方法.


```

@Controller
@RequestMapping("/user")
public class UserController2 {
    @RequestMapping(path="/handl31")
    public void handle31(OutputStream os) throws IOException{
        Resource res = new ClassPathResource("/image.jpg");
        FileCopyUtils.copy(res.getInputStream(),os);
    }
}


```


### 使用HttpMessageConverter<T>


![image](http://images0.cnblogs.com/blog/731047/201506/241347076276283.png)

HttpMessageConverter<T>是Spring的一个重要接口,它负责将请求信息转换为一个对象,(类型为T),将对象(类型为T)输出位响应信息.DispatcherServlet默认已经安装了RequestMappingHandlerAdapter的组件实现类,HTTPMessageConverter即由RequestMappingHandlerAdapter使用,将请求信息转换为对象,或将对象转换为响应信息.

HttpMessageConverter<T>接口定义了以下几个方法:

- boolean canRead(Class<?>clazz,MediaType mediaType):指定转换器可以读取的对象类型,即转换器可将请求信息转换为clazz类型的对象;同时指定支持的MIME媒体类型(如text/html,application/json)

- boolean canWrite(class<?>class,MediaType mediaType):指定转换器可以将class类型的对象写到响应流中,响应流支持的媒体类型在mediaType中定义.

- List<MediaType>getSupportedMediaTypes():该转换器支持的媒体类型

- T read(Class<? extends T> clazz,HttpInputMessage inputMessage):将信息流转换为T类型对象

- void write(T t,MediaType contentType ,HttpOutputMessage outputMessage):将T类型的对象写到响应流中,同时指定响应的媒体类型为contentType.

RequestMappingHandlerAdapter默认已经装配了以下HttpMessageConverter:

- StringHttpMessagerConverter
- ByteArrayHttpMessageConverter
- SourceHttpMessageConerter
- AllEncompassingFormHttpMessageConverer

使用HttpMessageConverter<T>将请求信息转换并绑定到处理方法的入参中呢:

- 使用@RequestBody /@responseBody 对处理方法进行标注

- 使用HttpEntity<T>/ResponseEntity<T>作为处理方法的入参或返回值.


```
@Controller
@RequestMapping(path="user")
public class UserController3 {
  @RequestMapping(path="/handl41")
    public String handle41(@RequestBody String requesBody){
      return  "success";
  }

    @ResponseBody
    @RequestMapping(path="handle42"/{imageId})
    public byte[] handle42(@PathVariable("imageId") String imageId)throws IOException{
        Resource res = new ClassPathResource("/image.jpg");
        byte[] fileData = FileCopyUtils.copyToByteArray(res.getInputStream());
        return fileData;
    }
}



```

handle41:将根据RequestBody入参标注一个@RequesBody注解,将根据RequestBody的类型查找匹配的HttpMessageConverter.由于StringHttpMessageConverter的泛型类型对应Srting,所以StringHttpMessageConverter将被选中,用它将请求体信息转换并将结果绑定到RequestBody上.

handle42()方法使用@ResponseBody注解,由于方法返回值类型为byte[],所以SpingMVC根据类型匹配的查找规则将使用ByteArrayHttpMessageConverter对返回值进行处理,将图片数据流输出到客户端.

结论:

- 当控制器处理方法使用@RequestBody/@ResponseBody或HttpEntity<T>/ResponseEntity<T>时,springMVC才使用注册的HttpMessageConverter对请求.响应消息进行处理

- 当控制器处理方法使用@RequestBody/@ResponseBody或或HttpEntity<T>/ResponseEntity<T>时,Spring首先根据请求头或者响应头的Accept属性选择匹配的HttpMessageConverer,然后根据参数类型或泛型类型的过滤得到匹配的HttpMessageConverter,如果如果找不到则报错

- @RequestBody和@ResponseBody不需要成对出现,如果方法入参使用了@RequestBody则使用HttpMessageCOnverter将请求消息转换并绑定到该入参中,如果方法标注了@ResponseBody,则选择匹配的HttpMessageConverter将方法返回值转换并输出响应消息.
