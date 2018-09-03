---
layout: post
title:  "PropertyPlaceholderConfigurer"
date:   2017-09-06 00:06:05
categories: bug
tags: spring
excerpt: bug记录
---

* content
{:toc}


### PropertyPlaceholderConfigurer 多properties文件的处理

#### 几个基础概念:

- 每调用完**一个**BeanFactoryPostProcessor之后，就会去解析**所有的bean中引用properties的占位符**,此时会出现不能解析后面PropertyPlaceholderConfigurer实例引入的占位符值
-   对于未解析到的占位符，可以通过order属性来调整bean装配的优先级，然后在最后装配的PropertyPlaceholderConfigurer实例上面启用未解析到的占位符检查。
-   忽略文件未找到:通过setIgnoreResourceNotFound()方法我们可以设置是否忽略文件未找到的情况，默认为false，即抛出异常信息。如果用户希望忽略对应的错误，则可以设置对应的值为true。
-   忽略变量不能解析:可以通过setIgnoreUnresolvablePlaceholders()方法指定ignoreUnresolvablePlaceholders的值为true，这样就将忽略变量不能被解析的情况。

```
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer"> 
   <property name="location" value="classpath:t12.properties"/> 
		<!-- 指定当外部属性文件不存在时不抛出异常 -->
		<property name="ignoreResourceNotFound" value="true"/>
		<!-- 忽略变量不能被解析的情况 -->
   <property name="ignoreUnresolvablePlaceholders" value="true"/>
</bean>

```
- 可以指定默认值:如果指定了属性变量的默认值，则在未找到可用于替换当前属性变量的属性时将使用定义好的默认值来替换当前属性变量。形式如：${varName:defValue}，其中varName表示变量名，defValue表示默认值。
- 当外部属性文件中的定义与内部属性的定义存在冲突，即存在相同的属性时，默认情况下是外部属性文件定义的属性值将覆盖内部properties定义的。如果需要设置内部属性定义将覆盖外部属性文件定义的属性，则可以通过setLocalOverride()方法指定对应的localOverride为true来达到设置内部属性定义覆盖外部属性文件定义的属性的目的。

```
     <!-- 通过setLocalOverride()方法设置内部属性定义将覆盖外部属性文件的定义 -->
	<bean
		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer"
		p:localOverride="true">
		
	</bean>
	
```
```
SYSTEM_PROPERTIES_MODE_NEVER：对应值为0。表示不使用系统属性进行替换。

SYSTEM_PROPERTIES_MODE_FALLBACK：对应的值为1，这是默认选项。表示只有在A类属性中没有找到变量对应的属性时才会尝试使用系统属性来进行替换。

SYSTEM_PROPERTIES_MODE_OVERRIDE：对应的值为2。表示当系统属性存在变量对应的属性时将使用系统属性的值。


```

- 多个 propertyConfigurer不会相互覆盖，但 如果同一个 propertyConfigurer中加载了多个配置文件，则后面的会覆盖前面先加载的的。
