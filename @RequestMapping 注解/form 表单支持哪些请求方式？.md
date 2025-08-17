---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[@RequestMapping 的 method 属性]]"
---
- 常用的请求方式有`GET`，`POST`，`PUT`，`DELETE`，但是目前浏览器只支持`GET`和`POST`（OS：刚才还有点疑惑的，这里好像“水落石出了”）

- 若在 form 表单提交时，为`method`设置了其他请求方式的字符串（`PUT`或`DELETE`），则按照默认的`GET`请求方式处理

- 若要发送`PUT`和`DELETE`请求，则需要通过 Spring 提供的过滤器`HiddenHttpMethodFilter`，在 restful 部分会讲到（OS：我上面刚自己研究了下，没想到老师这里会讲`^_^`）

如果去除过滤器`HiddenHttpMethodFilter`的配置，同时注释掉隐藏域的代码，并将`method`为`post`值改成`put`和`delete`

```html
<form th:action="@{/requestMappingController/success}" method="put">
    <!--    <input type="hidden" name="_method" value="put"/>-->
    <input type="submit" value="测试PutMapping">
</form>
<br/>
<form th:action="@{/requestMappingController/success}" method="delete">
    <!--    <input type="hidden" name="_method" value="delete"/>-->
    <input type="submit" value="测试DeleteMapping">
</form>
```

按照上述的说法，会按照默认的`GET`请求方式处理，这里进行验证

![[1755424292410_TrTw7xudo8.gif]]

可以发现，本应走`PUT`和`DELETE`方式的请求，都被`GET`请求方式的控制器方法处理了。如果这里控制器中连相应`GET`请求方式都没有定义的话，肯定是报 405 了。这里注释掉`@GetMapping`的请求方法

```java
//@GetMapping("/success")
//public String successGet() {
//    return "successget";
//}
```

验证结果：显而易见

![[1755424292410_mZLjsAYIMJ.gif]]