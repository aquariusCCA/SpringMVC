---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[RESTful 简介]]"
  - "[[使用 RESTful 模拟操作用户资源]]"
---
目前，我们在`web.xml`配置文件中对`CharacterEncodingFilter`和`HiddenHttpMethodFilter`的配置顺序如下

```xml
<!--处理编码-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 略 -->
<!--配置 org.springframework.web.filter.HiddenHttpMethodFilter: 可以把 POST 请求转为 DELETE 或 POST 请求 -->
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

如果，将两者顺序颠倒互换，即

```xml
<!--配置 org.springframework.web.filter.HiddenHttpMethodFilter: 可以把 POST 请求转为 DELETE 或 POST 请求 -->
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 略 -->
<!--处理编码-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

看看顺序改变之后，对请求的编码会有什么影响

![[1755504967896_H4n3yaAV5A.gif]]

后台日志信息

```shell
修改用户信息：User{username='å¼ ä¸�', password='123456', gender='male', age='18', email='123@qq.com'}
```

可以发现，中文的“张三”乱码了，这是为什么呢？

> 在 **SpringMVC 获取请求参数** 一节中的 [[处理乱码问题]] 中，我们尝试在获取请求参数之前，通过`request.setCharacterEncoding("UTF-8");`来设置请求编码格式，但是没有生效。分析的原因是 ==请求参数获取在前，设置编码格式在后== 导致的。
> 
> 我们也提出了 2 种解决方案：
> 
> 1、获取请求参数之后，手动解码编码。但是这种显然不合理，所以直接 pass
> 2、获取请求参数之前“做手脚”。这种方式就是 SpringMVC 中提供的`CharacterEncodingFilter`过滤器，来对请求编码做统一处理
> 
> 现在的问题就是：在`CharacterEncodingFilter`之前配置了`HiddenHttpMethodFilter`导致了失效
> 
> 所以我们需要搞清楚，为什么`CharacterEncodingFilter`的配置顺序会影响到编码的效果？或者说为什么`HiddenHttpMethodFilter`会使之失效？

通过上面对`HiddenHttpMethodFilter`源码的剖析，它会获得`_method`这个请求参数，这就导致执行到 `CharacterEncodingFilter` 过滤器时，已经是获取请求参数之后了，所以会导致上述中文乱码问题

因此，我们必须要将`CharacterEncodingFilter`过滤器尽量配置在其他过滤器之前。这样就能保证在任何过滤器获取请求之前，获得的失已经处理过编码格式的请求参数了

我们再将`CharacterEncodingFilter`和`HiddenHttpMethodFilter`的配置顺序还原至之前的状态，即`CharacterEncodingFilter`在前而`HiddenHttpMethodFilter`在后的情况，进行测试再查看后台日志信息

```shell
修改用户信息：User{username='张三', password='', gender='male', age='18', email='123@qq.com'}
```

这时，当请求参数中包含中文时，就不会出现乱码的情况了