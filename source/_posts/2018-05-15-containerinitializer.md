---
layout: post_layout
title: 「Spring」SpringMVC中的AbstractAnnotationConfigDispatcherServletInitializer说明
time: 2018年05月15日 星期二
location: 北京
pulished: true
excerpt_separator: "```"
---

#### **1.AbstractAnnotationConfigDispatcherServletInitializer**简单说明:
我们需要知道在SpringMVC项目中，扩展AbstractAnnotationConfigDispatcherServletInitializer的任意类都会自动的配置
DispatcherServlet和Spring应用上下文，Spring应用上下文会位于应用程序的Servlet上下文之中。

<!--more-->

```java
/**
 * @Description 自动配置spring上下文
 * @Author wind.
 * @Date 2018/5/14 10:15.
 * @Modified By:
 */
public class MyWebAppInnitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    /**
     * 配置DispatcherServlet应用上下文的bean
     * @return
     */
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{RootConfig.class};
    }

    /**
     * 配置ContextLoaderListener创建应用上下文的bean
     * @return
     */
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{WebConfig.class};
    }

    /**
     * 将DispatcherServlet映射到"/"
     * @return
     */
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

#### **2.AbstractAnnotationConfigDispatcherServletInitializer**作用剖析：
> 在Servlet3.0环境中，容器会在类路径中查找实现了javax.servlet.ServletContainerInitializer接口的类，
如果能发现的话，就用它来配置Servlet容器。
Spring提供了这个接口的实现，名为SpringServletContanerInitializer,这个类反过来又会查找实现WebApplicationInitializer的类
并将配置的任务交给他们来完成。Spring3.2引入了一个便利的WebApplicationInitializer基础实现，也就是AbstractAnnotationConfigDispatcherServletInitializer。
因为我们的MyWebAppInnitializer扩展了AbstractAnnotationConfigDispatcherServletInitializer（同时也就实现了WebApplicationInitializer）,
因此当部署到Servlet3.0容器（Tomcat7/8）的时候，容器会自动发现它，并用它来配置servlet上下文。

#### **3.SpringServletContanerInitializer**中查找**WebApplicationInitializer**的方法:
```java
@Override
public void onStartup(@Nullable Set<Class<?>> webAppInitializerClasses, ServletContext servletContext)
        throws ServletException {

    List<WebApplicationInitializer> initializers = new LinkedList<>();

    if (webAppInitializerClasses != null) {
        for (Class<?> waiClass : webAppInitializerClasses) {
            // Be defensive: Some servlet containers provide us with invalid classes,
            // no matter what @HandlesTypes says...
            if (!waiClass.isInterface() && !Modifier.isAbstract(waiClass.getModifiers()) &&
                    WebApplicationInitializer.class.isAssignableFrom(waiClass)) {
                try {
                    initializers.add((WebApplicationInitializer)
                            ReflectionUtils.accessibleConstructor(waiClass).newInstance());
                }
                catch (Throwable ex) {
                    throw new ServletException("Failed to instantiate WebApplicationInitializer class", ex);
                }
            }
        }
    }

    if (initializers.isEmpty()) {
        servletContext.log("No Spring WebApplicationInitializer types detected on classpath");
        return;
    }

    servletContext.log(initializers.size() + " Spring WebApplicationInitializers detected on classpath");
    AnnotationAwareOrderComparator.sort(initializers);
    for (WebApplicationInitializer initializer : initializers) {
        initializer.onStartup(servletContext);
    }
}
```

#### **4.WebApplicationInitializer**的继承结构：
![](/img/20180515/1.png){:height="100%" width="100%"}