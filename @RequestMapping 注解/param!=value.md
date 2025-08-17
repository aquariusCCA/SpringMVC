---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[params 属性]]"
---
这里将`params`的值指定为`username!=admin`，即要求请求中不仅要携带`username`的请求参数，且值不能为`admin`

```java
@RequestMapping(
    value = {"/testParams"},
    params = {"username!=admin"}
)
public String testParams() {
    return "success";
}
```

测试结果

![[1755424961425_1oSiTm31gM.gif]]

实际测试结果发现：不携带`username`请求参数的请求和携带`username`请求参数但值不为`admin`的请求，可以正常访问；而携带`username`请求参数但值为`admin`的请求，不能正常访问，不完全符合预期

> 存疑点：不携带`username`请求参数的请求能够正常访问，这一点不符合课程中讲解的内容，此点存疑