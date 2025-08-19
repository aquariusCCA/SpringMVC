---
up:
  - "[[Spring MVC 課程描述]]"
補充內容:
  - "[[Spring中对于WebApplicationInitializer的理解]]"
---
在 Servlet3.0 环境中，容器会在类路径中查找实现`javax.servlet.ServletContainerlnitializer`接口的类，如果找到的话就用它来配置 Servlet 容器

Spring 提供了这个接口的实现，名为`SpringServletContainerlnitializer`，这个类反过来又会查找实现`WebApplicationlnitializer`的类并将配置的任务交给它们来完成

Spring3.2 引入了一个便利的 `WebApplicationlnitializer`基础实现，名为`AbstractAnnotationConfigDispatcherServletlnitializer`

当类扩展了`AbstractAnnotationConfigDispatcherServletlnitializer`并将其部署到 Servlet3.0 容器时，容器会自动发现它，并用它来配置 Servlet 上下文