---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[params 属性]]"
---
这里将`params = {"username"}`中`username`前加上`!`即可，即`params = {"!username"}`，这就要求请求中的请求参数中不能携带`username`请求参数

```java
@RequestMapping(
    value = {"/testParams"},
    params = {"!username"}
)
public String testParams() {
    return "success";
}
```

测试结果

![[1755424961425_GGvkCL77Er.gif]]

可以发现，没有携带`username`请求参数的请求变得能够正常访问，而携带了`username`请求参数的请求反而出现了`400`的异常信息，符合预期

```js
HTTP Status 400：Parameter conditions "!username" not met for actual request parameters: username={admin}, password={123456}
```
