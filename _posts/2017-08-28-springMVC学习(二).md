---
layout: post
title:  "springMVC学习(二)"
date:   2017-06-10 00:06:05
categories: springMVC
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
