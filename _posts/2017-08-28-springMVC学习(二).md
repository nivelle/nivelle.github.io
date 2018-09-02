---
layout: post
title:  "springMVC学习(二)"
date:   2017-06-10 00:06:05
categories: spring
tags: springMVC
excerpt: springMVC
author: nivelle
---

* content
{:toc}


### 处理数据模型

方法入参绑定请求消息只是处理方法的第一步,还有更为重要的任务等待完成,根据入参执行相应的逻辑,产生模型数据,导向特定视图中.

SpringMVC提供了多种途径输出模型数据,如下:

- ModelAndView :当处理方法返回值类型为ModelAndView时,方法体即可通过该对象添加模型数据
- @ModelAttribute:在方法入参标注该注解后,入参的对象就会放到数据模型中
- Map及Model:如果方法入参为org.springframework.ui.Model,org.springframework.ui.ModelMap或java.util.Map,则当处理方法返回时,Map中的数据会自动添加到模型中.
- @SessionAttributes:将模型中的某个属性暂存到HttpSession中,便多个请求可以共享这个属性.

1. ModelAndView

可以简单将模型数据看作一个Map<String,Object>对象:

- ModelAndView addObject(String attributeName,Object attributeValue)
- ModelAndView addAllObjects(Map<String,?>modelMap)
- void setView(view view):指定一个具体视图对象
- void setViewName(String viewName):指定一个逻辑视图名.

2. @ModelAttribute

如果希望将方法入参对象添加到模型中,则仅需响应入参前使用@ModelAttribute注解即可.

```
@RequestMapping(path="/handle")
public String handle(@ModelAttribute("user") User user){
    user.setUserId("100");
    return "/user/createSuccess";
}

```
Spring MVC将请求消息绑定到对象中,然后以user为键,Usuer对象放到模型中,在准备对视图进行渲染前,SpringMVC还会进一步将模型中的数据转储到视图上下文中并暴露给视图对象.对于jsp,会将模型数据转储到servletRequest的书写列中,通过ServletRequest setAttribute(name,Object)保存.在视图中可通过${user.userName}访问模型中的数据.除了在入参上使用@ModelAttribute注解外,还可以在方法定义中使用.


SpirngMVC在调用目标处理方法前,会逐个调用在方法级别上标注了@ModelAttribute注解的方法,并将这些方法的返回值添加到模型中.

![image](http://7xpuj1.com1.z0.glb.clouddn.com/ModelAttribute.png)


3. Map以及Model

SpringMVC 在内部使用一个org.springframework.ui.Model接口存储模型数据,它们功能类似java.util.Map,但它比Map易用.ModelMap实现了Map接口.而org.springframework.ui.ExtendedModelMap扩展与ModelMap的同时实现了Model接口.

**SpirngMVC 在调用方法前会创建一个隐含的模型对象,作为模型数据的存储容器,我们称之为"隐含模型".如果处理方法的入参为Map或Model类型,则Sping MVC会将隐含模型的引用传递给这些入参.方法体内,开发者可以通过这个入参对象访问到模型中的所有数据,也可以向明显中添加信息属性数据.**

![image](http://7xpuj1.com1.z0.glb.clouddn.com/ModelMap.png)


SpringMVC一旦发现处理方法有Map或Model类型的入参,就会将请求在内的隐含模型对象传递给这些参数,因此在方法体中可以通过这个入参对模型中的数据进行读写操作.


4. @SessionAttributes

如果希望在多个请求之间公用某个模型属性数据,则可以在控制器类标注一个@SessionAttributes,SpringMVC会将模型中对应的属性暂存到HttpSession中.

![image](http://7xpuj1.com1.z0.glb.clouddn.com/TIM%E6%88%AA%E5%9B%BE20170830001820.png)

在(1)处标注的@AttributeAttributes("user")会自动将处理器中任何方法属性名为user的模型属性透明底存储到HttpSession中.(2)处,handle71()方法的User user入参会添加到隐含模型中,于是这个模型属性在handle71()方法执行时,会由Spring MVC将其透明底保存到HttpSession中.

在(3)处可以从HttpSession中获取属性值.

注意:一般情况下,控制器放回字符串类型的值会被当做逻辑视图名处理,但如果字符串带"forward:"或"redirect:"前缀,则Spring MVC将对它进行特殊处理:将"forward:"或"redirect:"当成指示符,其后字符串作为URL处理.

![image](http://7xpuj1.com1.z0.glb.clouddn.com/ModelAttributeAndSessionAttribute.png)


