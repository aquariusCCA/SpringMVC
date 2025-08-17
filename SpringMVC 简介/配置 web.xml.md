---
up:
  - "[[Spring MVC 課程描述]]"
---
> 为什么要配置`web.xml`？注册 SpringMVC 的前端控制器 DispatcherServlet

# 1）默认配置方式

此配置作用下，SpringMVC 的配置文件默认位于 WEB-INF 下，默认名称为 `<servlet-name>-servlet.xml`

例如，以下配置所对应 SpringMVC 的配置文件位于 WEB-INF 下，文件名为 `springMVC-servlet.xml`

```xml
<!--配置 SpringMVC 的前端控制器，对浏览器发送的请求统一进行处理-->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--
      设置 SpringMVC 的核心控制器所能处理的请求的请求路径
      / 所匹配的请求可以是 /login 或 .html 或 .js 或 .css 方式的请求路径
      但是 / 不能匹配 .jsp 请求路径的请求
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

默认配置方式对位置和名称都是默认的，这样并不好！Maven 工程配置文件应该统一放置在`resources`下，应该如何来实现呢？来看下面的“扩展配置方式”

---

# 2）扩展配置方式

- 通过`init-param`标签设置 SpringMVC 配置文件的位置和名称
- 通过`load-on-startup`标签设置 SpringMVC 前端控制器 DispatcherServlet 的初始化时间

```xml
<!--配置 SpringMVC 的前端控制器，对浏览器发送的请求统一进行处理-->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--通过初始化参数指定SpringMVC配置文件的位置和名称-->
    <init-param>
        <!--contextConfigLocation 为固定值-->
        <param-name>contextConfigLocation</param-name>
        <!--使用 classpath: 表示从类路径查找配置文件，java 工程默认src下,maven 工程默认 src/main/resources 下-->
        <param-value>classpath:springMVC.xml</param-value>
    </init-param>
    <!--
        作为框架的核心组件，在启动过程中有大量的初始化操作要做
        而这些操作放在第一次请求时才执行会严重影响访问速度
        将前端控制器 DispatcherServlet 的初始化时间提前到服务启动时
    -->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--
        设置 SpringMVC 的核心控制器所能处理的请求的请求路径
        / 所匹配的请求可以是 /login 或 .html 或 .js 或 .css 方式的请求路径
        但是 / 不能匹配 .jsp 请求路径的请求
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

> [!NOTE] `<url-pattern>` 标签中使用`/`和`/*`的区别：
> 
> `/`所匹配的请求可以是`/login`或`.html`或`.js`或`.css`方式的请求路径，但是`/`不能匹配`.jsp`请求路径的请求
>
> 因此就可以避免在访问`.jsp`页面时，该请求被 DispatcherServlet 处理，从而找不到相应的页面的情况
>
> `/*`则能够匹配所有请求，例如在使用过滤器时，若需要对所有请求进行过滤，就需要使用`/*`的写法
