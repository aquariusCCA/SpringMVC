---
up:
  - "[[Spring MVC 課程描述]]"
---
查看`@RequestMapping`注解的源码，可以清楚地看到`@Target`中有`TYPE`和`METHOD`两个属性值，表示其可以作用在类或方法上

![[1755333753420_CSMD6v7Q0R.png]]

也就是说`@RequestMapping`不仅可以在控制器方法上进行使用，也可以在控制器类上进行使用。那两种方式有什么区别呢？

- `@RequestMapping`标识一个类：设置映射请求的请求路径的初始信息
- `@RequestMapping`标识一个方法：设置映射请求的请求路径的具体信息

```java
@Controller
@RequestMapping("/requestMappingController")
public class RequestMappingController {
    @RequestMapping("/success")
    public String success() {
        return "success";
    }
}
```

前台创建超链接，超链接跳转路径 = 控制器上`@RequestMapping`映射请求的初始路径 + 控制器方法上`@RequestMapping`映射请求的具体路径，即`/requestMappingController/success`，再将其使用`Tymeleaf`的`@{}`语法包裹起来，这样`Tymeleaf`会为我们自动加上上下文路径

```html
<a th:href="@{/requestMappingController/success}">访问指定页面success.html</a>
```

测试结果

![[1755333753420_ReDh1UccIZ.gif]]

这样就可以为不同的控制器方法，在设置映射请求的请求路径时，指定不同的初始信息，从而避免**控制器中有多个方法对应同一个请求的情况**，这也就解决了之前的问题