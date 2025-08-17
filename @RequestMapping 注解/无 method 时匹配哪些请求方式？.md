---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[@RequestMapping 的 method 属性]]"
---
通过测试验证猜想

```html
<a th:href="@{/requestMappingController/test}">测试RequestMapping注解的method属性-->GET</a>
<br/>
<form th:action="@{/requestMappingController/test}" method="post">
    <input type="submit" value="测试RequestMapping注解的method属性-->POST">
</form>
```

测试结果：事实证明，控制器方法不添加`method`属性时，可以接收`GET`和`POST`的请求，那么应该是默认不对请求方式限制了

![[1755422673275_yzmUkQtEyi.gif]]

本着严谨的态度，再测试下是否在不添加`method`属性时，默认也支持`PUT`和`DELETE`请求

不过，`PUT`和`DELETE`请求比较特殊，需要使用到隐藏域，且`method`固定为`POST`

```html
<form th:action="@{/requestMappingController/test}" method="post">
    <input type="hidden" name="_method" value="put"/>
    <input type="submit" value="测试RequestMapping注解的method属性-->PUT">
</form>
<br/>
<form th:action="@{/requestMappingController/test}" method="post">
    <input type="hidden" name="_method" value="delete"/>
    <input type="submit" value="测试RequestMapping注解的method属性-->DELETE">
</form>
```

同时在`web.xml`中需要添加隐藏域请求方式的过滤器配置

```xml
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/</url-pattern>
</filter-mapping>
```

测试结果也是成功的

![[1755422673275_Gu1CrDbFOU.gif]]

