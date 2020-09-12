---
slug: spring-in-action-notes
title: Spring 实战笔记
author: Guan Chao
author_url: https://github.com/levid-gc
author_image_url: https://avatars1.githubusercontent.com/u/6498582?s=60&v=4
tags: []
---

## 第 1 章：Spring 起步

任何实际的应用程序都是由很多组件组成的，每个组件负责整个应用功能的一部分，这些组件需要与其他的应用元素进行协调以完成自己的任务。当应用程序运行时，需要以某种方式创建并引入这些组件。

Spring 的核心是提供了一个容器（container），通常称为 Spring 应用上下文（Spring application context），它们会创建和管理应用组件。这些组件也可以称为 bean，会在 Spring 应用上下文种装配在一起，从而形成一个完整的应用程序。就像砖块、砂浆、木材、管道和电线组合在一起，形成一栋房子似的。

将 bean 装配在一起的行为是通过一种基于依赖注入（dependency injection, DI）的模式实现的。此时，组件不会再去创建它所依赖的组件并管理它们的生命周期，使用依赖注入的应用依赖于单独的实体（容器）来创建和维护所有的组件，并将其注入到需要它们的 bean 种。通常，这是通过构造器参数和属性访问方法来实现的。

在核心容器之上，Spring 及其一系列的相关库提供了 Web 框架、各种持久化可选方案、安全框架、与其他系统集成、运行时监控、微服务支持、反应式编程以及总舵现代应用开发所需的特性。

在历史上，指导 Spring 应用上下文将 bean 装配在一起的方式是使用一个或者多个 XML 文件（描述各个组件以及它们与其他组件的关联关系）。

但是，在最近的 Spring 版本种，基于 Java 的配置更为常见，比如：

```java
@Configuration
public class ServiceConfiguration {
  @Bean
  public InventoryService inventoryService() {
    return new InventoryService();
  }

  @Bean
  public ProductService productService() {
    return new ProductService();
  }
}
```

`@Configuration` 注解会告知 Spring 这是一个配置类，会为 Spring 应用上下文提供 bean。这个配置类的方法使用 `@Bean` 注解进行了标注，表明这些方法所返回的对象以 bean 的形式添加到 Spring 的应用上下文中。

在 Spring 技术中，自动配置起源于所谓的自动装配（autowiring）和组件扫描（component scanning）。借助组件扫描技术，Spring 能够自动发现应用类路径下的组件，并将它们创建成 Spring 应用上下文中的 bean。借助自动装配技术，Spring 能够自动为组件注入它们所依赖的其他 bean。

最近，随着 Spring Boot 的引入，自动配置的能力以及远远超出了组件扫面和自动装配。Spring Boot 是 Spring 框架的扩展，提供了很多增强生产效率的方法。最为大家所熟知的增强方法就是自动配置（autoconfiguration），Spring Boot 能够基于类路径中的条目、环境变量和其他因素合理猜测需要配置的组件并将它们装配在一起。

<hr />

**1.2.2 检查 Spring 项目的结构**：

最强大的一行代码也是最短的：`@SpringBootApplication`，它是一个组合注解，组合了 3 个其他的注解。

- `@SpringBootConfiguration`：将该类声明为配置类。尽管这个类目前还没有太多的配置，但是后续我们可以按需添加基于 Java 的 Spring 框架配置。这个注解实际上是 `@Configuration` 注解的特殊形式。
- `@EnableAutoConfiguration`：启用 Spring Boot 的自动配置，告诉 Spring Boot 的自动配置他认为我们会用到的组件。
- `@ComponentScan`：启用组件扫描。这样我们能够通过像 `@Component`、`@Controller`、`@Service` 这样的注解声明其他类，Spring 会自动发现它们并将它们注册为 Spring 应用上下文中的组件。

<hr />

**1.3.1 处理 Web 请求**：

`@Controller` 并没有做太多的事情。它的主要目的是让组件稻苗将这个类识别为一个组件。因为 `HomeController` 带有 `@Controller`，所以 Spring 的组件扫描功能会自动发现它，并创建一个 `HomeController` 实例作为 Spring 应用上下文中的 bean。

实际上，有一些其他的注解与 `@Controller` 有着类似的目的（包括 `@Component`、`@Service` 和 `@Repository`）。你可以为 `HomeController` 添加上述的任意其他注解，其作用是完全相同的。但是，在这里选择使用 `@Controller` 更能描述这个组件在应用中的角色。



