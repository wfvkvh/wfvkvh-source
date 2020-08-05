---
layout: post_layout
title: 「Springboot」Springboot启动程序简单介绍
date: 2018-05-16
location: 北京
pulished: true
excerpt_separator: "```"
---

一个创建好的SpringBoot项目，我首先看到的可能是@SpringBootApplication注释，这个注释和以往的Spring注释有何区别呢？

<!--more-->

### 开启组件扫描和自动配置

@SpringBootApplication开启了Spring的组件扫描和Spring Boot的自动配置功能。实际上，@SpringBootApplciation将三个有用的注解组合到
了一起。
+ Spring的@Configuration：标明该类使用Spring基于Java的配置。
+ Spring的@ComponentScan：启用组件扫描，这样你写的Web控制器和其他组件才能被自动发现并注册为Spring应用程序上下文里的Bean。
+ Spring Boot的@EnableAutoConfiguration：这个不起眼的小注解也可以称为是一个咒语，就是这一行配置开启了Springboot自动配置的魔力，让你不用再写成篇的配置了。

### 配置文件
Initializer自动生成的application.properties文件可有可无，如果没有需要，删除也不会影响应用程序的运行。
如果向该文件中添加:
`server.port = 8000`
那么嵌入式的Tomcat的监听端口旧变成了8000，而不是默认的8080。
这说明application.properties文件可以很方便的帮你细粒度的调整Spring Boot的自动配置，还可以用来
指定应用程序代码所需要的配置项。    

### Gradle依赖
在Gradle里，dependencies任务会显示一颗依赖树，其中包含了项目所用的每一个库，一级他们的版本。
```bash
$ gradle dependencies
```

maven也有相似的功能
```bash
mvn dependency:tree
```

### Spring的条件化配置
Spring4.0 引入的新内容
当你注册bean时，可以对这个bean添加一定的自定义条件，当满足这个条件时，注册这个bean，否则不注册。
条件化最重要的是Condition接口，如下定义。   
```java
public interface Condition {
    boolean matches(ConditionContext var1, AnnotatedTypeMetadata var2);
}
```

实现这个接口，重写matches方法，返回true则意味着注册被注解的bean，false则跳过被注解的bean。
举个栗子：我们的系统中，如果注册了Cat类，就不要注册Dog类了。
```java
public class Person {

    private String name;

    @Autowired
    private  Animal animal;

    public Animal getAnimal() {
        return animal;
    }

    public void setAnimal(Animal animal) {
        this.animal = animal;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

}

public class Animal {

    private String name;

    public String getName() {
        return getClass().getName();
    }

    public void setName(String name) {
        this.name = name;
    }
}

public class Cat extends Animal{
}

public class Dog extends Animal{
}

@Configuration
public class BeanConfig {

    @Bean
    public Person person(){
        return new Person();
    }

    @Bean
    public Cat cat(){
        return new Cat();
    }

    @Bean
    //创建这个bean的必要条件,matches方法必须为true才会注册bean，反之忽略bean。
    //扫描到bean立刻调用，不会等配置类中的所有bean都注册好之后再调用，因此Cat bean写前面，写后面会出错。
    @Conditional({PetCondition.class})
    public Dog dog(){
        return new Dog();
    }

}

public class PetCondition implements Condition {

    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {


        //输出 org.springframework.core.type.StandardMethodMetadata
        System.out.println(annotatedTypeMetadata.getClass().getName());

        //强转
        StandardMethodMetadata standardMethodMetadata = (StandardMethodMetadata)annotatedTypeMetadata;

        //AppConfig下的dog方法调用，输出dog
        System.out.println(standardMethodMetadata.getMethodName());

        //如果person
        return conditionContext.getRegistry().containsBeanDefinition("person") && !conditionContext.getRegistry().containsBeanDefinition("cat");

        //conditionContext.getRegistry()获取注册的bean

        //conditionContext.getBeanFactory()获取提供bean definition的解析,注册功能,再对单例来个预加载(解决循环依赖问题).

        //conditionContext.getEnvironment()获取环境配置

        //conditionContext.getResourceLoader()ResourceLoader所加载的资源

        //conditionContext.getClassLoader()获取类加载器

        //annotatedTypeMetadata.
        //annotatedTypeMetadata 被加上注解的源数据信息。比如annotatedTypeMetadata.

    }

}

//测试代码
ApplicationContext context = new AnnotationConfigApplicationContext(BeanConfig.class);
        Person person = (Person)context.getBean("person");
        System.out.println(person.getAnimal().getName());  

```
明明配置类中注册了Animal的两种实现类，并且我们没有缩小bean的定义，然而并没有报错。这是因为在注册Dog的时候我们做了条件化判断，发现已经注册了Cat，因此跳过了Dog的注册，所以整个系统中Animal的子类只有Cat，Person类成功注入Cat。

