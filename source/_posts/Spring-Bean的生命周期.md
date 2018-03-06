---
title: Spring-Bean的生命周期
date: 2017-01-10 10:31:20
tags:
- spring
- bean-lifecycle
categories:
- spring

---

### BeanFactory中bean的生命周期

![](/images/beanfactory.png)

##### 各种接口方法分类
 
Bean的完整生命周期经历了各种方法调用，这些方法可以划分为以下几类：


###### 1、Bean自身的方法

包括了Bean本身调用的方法和通过配置文件中<bean>的init-method和destroy-method指定的方法

###### 2、Bean级生命周期接口方法

包括了BeanNameAware、BeanFactoryAware、InitializingBean和DiposableBean这些接口的方法

###### 3、容器级生命周期接口方法

包括了InstantiationAwareBeanPostProcessor 和 BeanPostProcessor 这两个接口实现，一般称它们的实现类为“后处理器”

InstantiationAwareBeanPostProcessor 接口本质是BeanPostProcessor的子接口，一般我们继承Spring为其提供的适配器类InstantiationAwareBeanPostProcessorAdapter来使用它

### Application中bean的生命周期

![](/images/application.png)

同beanFactory中bean的生命周期的区别在于

###### 1、Bean级生命周期接口方法

除了包括了BeanNameAware、BeanFactoryAware、InitializingBean和DiposableBean这些接口的方法，多加了ApplicationContextAware接口

###### 2、工厂后处理器接口方法

包括了BeanFactoryPostProcessor或者CustomEditonConfiurer, PropertyPlaceholderConfigurer等等非常有用的工厂后处理器接口的方法。工厂后处理器也是容器级的。在应用上下文装配配置文件之后调用一次。

如果配置文件中定义了多个工厂后处理器，最后让他们实现Ordered接口，以便Spring确定调用他们的顺序。

> ApplicationContext和BeanFactory另一个最大的不同之处在于：
> ApplicationContext会利用Java反射机制自动识别出配置文件中定义的
> BeanPostProcessor，InstantiationAwareBeanPostProcessor和
> BeanFactoryPostProcessor，并自动将他们注册到应用上下文中；而
> BeanFactory需要在代码中通过手工调用addBeanPostProcessor()方法> 进行注册。