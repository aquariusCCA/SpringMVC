---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[params 属性]]"
---
这里`params`的值指定为`username=admin`的形式，即要求请求中不仅要携带`username`的请求参数，且值为`admin`

```java
@RequestMapping(
    value = {"/testParams"},
    params = {"username=admin"}
)
public String testParams() {
    return "success";
}
```

测试结果

![[1755424961425_3NWhNz3Akk.gif]]

> 可以发现，不携带`username`请求参数的请求和携带`username`请求参数但不为`admin`的请求，均提示`400`的请求错误，符合预期