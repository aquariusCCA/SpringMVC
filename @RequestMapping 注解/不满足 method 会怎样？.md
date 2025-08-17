---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[@RequestMapping 的 method 属性]]"
---
**值得注意的是**

若当前请求的请求地址满足请求映射的`value`属性，但是请求方式不满足`method`属性，则浏览器报错

```js
HTTP Status 405 - Request method 'POST' not supported
```

![[1755422673275_YupXxNmpnX.png]]

代码验证

```java
@RequestMapping(value = {"/success", "/test"}, method = RequestMethod.GET)
public String success() {
    return "success";
}
```

验证结果：确实报了 405。同理可以将`method`属性值中`GET`改成`POST`、`PUT`和`DELETE`对应进行验证即可，这里就不一一验证了

![[1755422673275_vrlfmbq0kn.png]]