---
layout: post
title:  "spring之bean"
date:   2017-05-13 00:06:05
categories: 技术
tags: spring
excerpt: springBean
---


* content
{:toc}

###  容器启动流程



![image](http://7xpuj1.com1.z0.glb.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170513112138.png)

- 启动阶段

  加载ConfigurationMetadata(/xml),然后根据这些信息绑定整个系统对象，工具类（beanDefinitionReader）解析和分析，然后编组为相应的BeanDefinition,最后报保存了bean定义必要信息的BeanDefinition,注册到相应的DeanDefinitionRegister。


- bean实例化阶段

  当某个请求通过容器的getBean方法获取某个对象，就会根据BeanDefinition所提供的信息实例化被请求对象，并为其依赖注入。
  

第一阶段是根据图纸装配生产线，第二阶段就是使用装配好的生产线来生产具体产品。




###  BeanFactoryPostProcessor

该功能允许我们在容器实例化之前，对注册到容器的BeanDefinition所保存的信息做相应修改。

- 手动装配BeanFactory使用BeanFactoryPostProcessor
  
 ```
  //申明将被后处理的BeanFactory
  
  ConfigurableListableBeanFactory beanFactory = new XmlBeanFactory(new ClassPathResource("..."));
  
  //申明要使用的BeanFactoryPostProcessor
  
  PropertyPlaceholderConfigurer propertyPostProcessor = new PropertyPlaceholderConfigurer();
  
  propertyPostProcessor.setLocation(new ClassPathResource("..."));
  
  //执行后处理操作
  
  propertyPostProcessor.postProcessBeanFactory(beanFactory);
  
 ```
  
- ApplicationContext 使用BeanFactoryPostProcessor

 ```
  <beans>
     /<bean class="org.springframework.bean.factory.config.PropertyPlaceholderConfigurer">
       /<property name ="locations">
          /<list>
            <value>conf/jdbc.properties</value>
            <value>conf/mail.properties</value>
          </list>
       
       </property>
    /</bean>
  </beans>
  
 ```

---

#### spring提供的常用的BeanFactoryPostProcessor

- PropertyPlaceholderConfigurer

  它允许我们在xml配置文件中使用占位符（PlaceHolder）,并将这些占位符所代表的资源单独配置到简单的properties文件中来加载。
  
  PropertyPlaceholderConfigurer 不单会从其配置的properties文件中加载配置项，同时还会检查java的System类中的Properties，可以通过setSystemPropertiesMode()或者setSystemPropertiesModeName()来控制是否加载或者覆盖system相应的Properties行为。
  
- PropertyOverrideConfigurer

  可以通过PropertyOverrideConfigurer 对容器中配置的任何你想处理的bean定义的property信息进行替换。
  
  以PropertyOverrideConfigurer 的配置文件覆盖PropertyPlaceholderConfigurer中的配置项
  
- CustomEditorConfigurer
  
  它只是辅助性地将后期会用到的信息注册到容器，对BeanDefinition没有任何变动。

  xml对对象类型的描述都是用字符串形式的，最终应用程序却是用各种类型的对象构成，想要完成这种由字符串到具体对象的转换（不管谁来具体执行这个转换），都需要这种规则的相关的信息，而CustomEditorConfigurer就算传递这些信息的。
  
  spring内部通过javaBean的propertyEditor来帮助进行String类型到其他类型的转换工作。只要为每种类型提供一个PropertyEditor.
  
spring框架提供了自身实现的一些propertyEditor

  1. StringArrayPropertyEditor：  将符合CSV格式的字符串转换成String[]数组形式。ByteArraryPropertyEditoe、CharArrayPropertyEditor类似
  2. classEditor： 根据String类型的class名称，直接将其转化成相应的class对象，相当于class.forname()的功效
  3. FileEditor ：提供对java.io.file的propertyEditor.
  4. localeEditor:针对java.util.Locale.
 

自定义类型propertyEditor 举例

假设有如下依赖关系：
 
  ```
   public class DateFoo {
  
      private Date date;

      public Date getDate() {
        return date;
       }

      public void setDate(Date date) {
        this.date = date;
      }
  }

  ```
配置类似于

```
 /<bean id="dateFoo" clss="...DateFoo">
     /<property name="date">
          <value>2007/10/16</value>
     </property>

 </bean>



```

因为没有支持这样日期类型的editorProperty,需自定义：

- 直接继承java.beans.PropertyEditorSupport类避免实现java.beans.PropertyEditor.   
    
    假设要对yyyy/MM/dd形式的日期格式转换提供支持。

   ![image](http://7xpuj1.com1.z0.glb.clouddn.com/editorproperty.png)


- 通过CustomEditorConfigurer 注册自定义的PropertyEditor

  如果是BeanFactory实现：
  
```
XmlBeanFactory beanFactory = new XmlBeanFactory(new ClassPathResource("..."));

CustomEditorConfigurer ceConfigurer = new CustomEditorConfigurer();
Map customerEditors = new HashMap();
customerEditors.put(java.util.date.class,new DatePropertyEditor());
ceConfigurer.setCustomEditors(customerEditors);

ceConfigurer.postProcessBeanFactory(beanFactory);

```
  
  如果是ApplicationContext,因为会自动识别FactoryPostProcessor.

```
  <bean class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <property name="cusomeEditors">
        <map>
           <entry key ="java.util.Date">
               <ref bean ="DatePropertyEditor"/>
           </entry>
        </map>
    </property>
  </bean>


```  


---


spring2.0之前通过CustomEditorConfigurer的customEditors属性来指定自定义的PropertyEditor. 2.0之后，提倡使用propertyEditorRegistrars属性来自定自定义的propertyEditor.不过需要先定义一个org.springframework.beans.PropertyEditorRegistrar.

 ![image](http://7xpuj1.com1.z0.glb.clouddn.com/DatePropertyEditorRegistrar.png)
 
 ![image](http://7xpuj1.com1.z0.glb.clouddn.com/%E8%87%AA%E5%AE%9A%E4%B9%89propertyEditor.png)
 
---
 
####   bean生命周期

---

![image](http://7xpuj1.com1.z0.glb.clouddn.com/beanLife.png)

容器启动时，容器仅仅保留BeanDefinition来保存实例化阶段必要的信息，只有在调用getBean()时候才真正启动实例化阶段活动。


1. bean的实例化与BeanWrapper 

   容器内部实现bean采用“策略模式”，可以通过反射或者CGLIB动态字节码生成来初始化相应的bean。
   
   实例化结果返回的是包装类，BeanWrapper

 2 .BeanWrapper 它通常在Spring框架内部使用，他有个实现类org.springframework.beans.BeanWrapperImpl. 他主要对某个bean进行包裹，然后对这个包裹的bean进行操作，比如设置或获取bean的相应属性值。

    BeanWrapper继承了...PropertyAccessor接口可以以统一方式对对象属性进行访问；同时继承了PropertyEditorRegistry和TypeConverter接口。使用BeanWrapper对bean操作更方便，可以免去使用java反射API。

 ---
 beanFactory中当对象实例化完成并且相关属性以及依赖设置完成之后，Spring容器会检查当前对象实例是否实现了一系列的Aware命名结尾的接口定义，如果是，则将这些接口定义的规定依赖注入给当前对象实例。

 例如：BeanFactoryAware,会将自身设置到当前对象实例，这样当前对象就拥有了一个BeanFactory容器引用，且可以对这个容器内允许访问的对象按照需要 进行访问。


 Application中使用的是BeanPostProcessor方式：

 与BeanFactoryPostProcessor通常会处理容器内所有符合条件的BeanDefinition类型，BeanPostProcessor会处理容器内所有符合条件的实例化的对象实例。

 ```
 public interface BeanPostProcessor
 {
     Object postProcessBeforeInitialization(obejct bean,string beanname) throws BeansException;//前置处理

     Object postProcessAfterInitialization(obejct bean,string beanname) throws BeansException;//后置处理
 }


 ```

3.BeanPostProcessor 处理标记接口实现类，或者为当前对象提供代理实现。第三步中,applicationContext对应的那些Aware接口实际上就是通过BeanPostProcessor的方式处理。

当applicationContext每个对象实例化走到BeanPostProcessor前置处理这一步是，applicationContext会检测到注册到容器的ApplicationContextAwareProcessor这个BeanPostProcessor的实现类，然后调用postProcessBeforeInitialization方法，检测设置Aware相关依赖。


4.Initializingbean和Init-method

```
public interface InitializingBean{
   void afterPropertiesSet() throws Exception;
}

```
在对象实例化过程中调用过“BeanPostProcessor的前置处理”之后，检测是否实现了InitializingBean接口，若是，则调用afterPropertiesSet（）方法进一步调整对象实例状态。替代方法：\<init-method>


5.DisposableBean与destroy_method

所有的一切操作完成之后，容器将检查singleton类型的bean实例，查看是否实现disposableBean接口。或是destroy-method指定了销毁方法，若是就会为该实例注册一个用于对象销毁的回调，以便singleton类型的对象实例销毁之前，执行销毁逻辑。
 
