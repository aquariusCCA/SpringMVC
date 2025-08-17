---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[SpringMVC 视图]]"
---
当前请求映射对应的控制器方法中，仅仅用来实现页面跳转，而没有其他请求过程的处理，即只需设置一个视图名称时，就可以将控制器方法使用`view-controller`标签进行表示

例如：我们在`HelloController`中配置的一个控制器方法，对应`view`请求，返回`view`视图

```java
@RequestMapping("/view")
public String view() {
    return "view";
}
```

此时通过在SpringmMVC 配置文件中添加`<mvc:view-controller>`标签，就可以代替上述控制器方法（将上述方法注释即可）

```xml
<mvc:view-controller path="/view" view-name="view"></mvc:view-controller>
```

其中

- `path`对应控制器方法上`@RequestMapping`中路径
- `view-name`对应控制器方法返回的视图名称

此时再来访问`/view`，同样会被`Thymeleaf`视图解析器解析，拼接上视图前缀和视图后缀后，找到对应路径下的`view.html`页面

> **注意**：在 SpringMVC 配置文件中配置了`view-controller`之后，控制器中所有的请求映射都会失效

测试结果

![[1755431563406_9gCVclxTgA.gif]]

怎么解决这个问题呢？我们需要在 SpringMVC 配置文件中开启 MVC 的注解驱动

```xml
<!--
    当SpringMVC中设置任何一个view-controller时，其他控制器中的请求映射将全部失效，
    此时需要在SpringMVC的核心配置文件中设置开启mvc注解驱动的标签：
    -->
<!--开启 MVC 的注解驱动-->
<mvc:annotation-driven/>
```

测试结果

![[1755432634073_U8jox9D6lu.gif]]


> [!NOTE] **额外的**：MVC 的注解驱动功能很多，例如
> 
> 1、如果加上了默认的 Servlet 处理静态资源（如 JS、CSS 等），控制器请求映射会失效，这时需要配置 MVC 的注解驱动
> 
> 2、JAVA 对象转换为 JSON 对象，同样需要配置 MVC 的注解驱动
> 
> 因为使用场景很多，所以一般情况下 MVC 注解驱动默认是需要配置的。但是注意，需要了解在不同情况下 MVC 注解驱动的功能是什么
