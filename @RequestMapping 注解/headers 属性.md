---
up:
  - "[[Spring MVC 課程描述]]"
---
`@RequestMapping`注解的`headers`属性通过请求的请求头信息匹配请求映射

它是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系

- `header`：要求请求映射所匹配的请求必须携带`header`请求头信息
- `header`：要求请求映射所匹配的请求必须不能携带`header`请求头信息
- `header=value`：要求请求映射所匹配的请求必须携带`header`请求头信息且`header=value`
- `header!=value`：要求请求映射所匹配的请求必须携带`header`请求头信息且`header!=value`

若当前请求满足`@RequestMapping`注解的`value`和`method`属性，但是不满足`headers`属性，此时页面显示`404`错误，即资源未找到

测试代码

```java
@RequestMapping(
    value = {"/testHeaders"},
    headers = {"Host=localhost:8081"}
)
public String testHeaders() {
    return "success";
}
```

测试结果

![[1755424961425_RWJa8WydLy.gif]]

因为我本地`tomcat`启动端口是`8080`，所以是匹配不成功的，此时显示`404`错误，符合预期

再将端口号修改为`8080`

```java
@RequestMapping(
    value = {"/testHeaders"},
    headers = {"Host=localhost:8080"}
)
public String testHeaders() {
    return "success";
}
```

测试结果

![[1755425800115_XloCHIQCip.gif]]

这一次，因为端口号一致，所以成功跳转，符合预期