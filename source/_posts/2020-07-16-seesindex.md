---
title: 「Spring」Spring实现的EE规范和Spring的设计哲学 
date: 2020-07-16 17:26:14
tags: 
- Spring
---


## JSR 规范

Spring 出现于2003年，是对早期 J2EE 规范的复杂性的响应。虽然有些人认为 javaee 和 Spring 是竞争对手，但实际上 Spring 是 javaee 的补充。Spring 编程模型并不包含 Java EE 平台规范; 相反，它集成了来自 EE 平台的精心挑选的个别规范: 

<!--more-->

> Concurrency Utilities (JSR 236)

> Common Annotations (JSR 250) 

> Bean Validation (JSR 303)

> Dependency Injection (JSR 330) 

> JPA (JSR 338)

> Servlet API (JSR 340)

> WebSocket API (JSR 356)

> JSON Binding API (JSR 367)

> JMS (JSR 914)

> as well as JTA/JCA setups for transaction coordination, if necessary.

## Spring 的设计哲学

When you learn about a framework, it’s important to know not only what it does but what principles it follows. Here are the guiding principles of the Spring Framework:

* 当你了解一个框架时，重要的是不仅要知道它是做什么的，还要知道它遵循什么原则。以下是 Spring 框架的指导原则:

* Provide choice at every level. Spring lets you defer design decisions as late as possible. For example, you can switch persistence providers through configuration without changing your code. The same is true for many other infrastructure concerns and integration with third-party APIs.

* 在每个层面提供选择。Spring 允许您尽可能晚地推迟设计决策。例如，您可以通过配置切换持久性提供程序，而无需更改代码。许多其他基础设施关注点和与第三方 api 的集成也是如此。

* Accommodate diverse perspectives. Spring embraces flexibility and is not opinionated about how things should be done. It supports a wide range of application needs with different perspectives.

* 容纳不同的观点。Spring 支持灵活性，不固执己见地认为应该如何做事。它以不同的视角支持广泛的应用需求。

* Maintain strong backward compatibility. Spring’s evolution has been carefully managed to force few breaking changes between versions. Spring supports a carefully chosen range of JDK versions and third-party libraries to facilitate maintenance of applications and libraries that depend on Spring.

* 保持强壮的向下兼容。Spring 的演变经过了精心的管理，在不同版本之间几乎没有突破性的变化。Spring 支持精心选择的一系列 JDK 版本和第三方库，以促进依赖于 Spring 的应用程序和库的维护。

* Care about API design. The Spring team puts a lot of thought and time into making APIs that are intuitive and that hold up across many versions and many years.

* 关心 API 设计。Spring 团队花费了大量的思想和时间来制作直观的 api，这些 api 可以支持多个版本和多年。

* Set high standards for code quality. The Spring Framework puts a strong emphasis on meaningful, current, and accurate javadoc. It is one of very few projects that can claim clean code structure with no circular dependencies between packages.

* 为代码质量设置高标准。Spring 框架强调有意义的、当前的和准确的 javadoc。它是少数几个可以声称代码结构清晰、包之间没有循环依赖关系的项目之一。