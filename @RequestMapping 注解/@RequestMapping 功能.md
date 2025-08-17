---
up:
  - "[[Spring MVC 課程描述]]"
---
从注解名称上我们可以看到，`@RequestMapping`注解的作用就是 ==将请求和处理请求的控制器方法关联起来，建立映射关系==

SpringMVC 接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理这个请求

# **控制器中有多个方法对应同一个请求的情况**

这是一种特殊情况，我们定义至少两个控制类，其中定义的控制器方法上`@ReqeustMapping`指定同一请求路径

```java
@Controller
public class HelloController {
    @RequestMapping("/")
    public String index() {
        return "index";
    }
}
@Controller
public class RequestMappingController {
    @RequestMapping("/")
    public String index() {
        return "target";
    }
}
```

如果存在两个或多个控制器，其控制器方法的`@RequestMapping`一致，即多个控制器方法试图都想要处理同一个请求时，这时启动 Tomcat 时会抛出`BeanCreationException`异常，并指明`There is already 'helloController' bean method`即 helloController 中存在处理同意请求的控制器方法

```java
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping': Invocation of init method failed; nested exception is java.lang.IllegalStateException: Ambiguous mapping. Cannot map 'requestMappingController' method 
com.vectorx.springmvc.s00_helloworld.RequestMappingController#index()
to { [/]}: There is already 'helloController' bean method
com.vectorx.springmvc.s00_helloworld.HelloController#index() mapped.
```

但是这显然是不合理的，当一个系统方法很多，很难完全避免两个或多个同名方法的存在，那该怎么办呢？