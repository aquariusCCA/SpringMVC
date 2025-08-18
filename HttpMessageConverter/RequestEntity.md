---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[HttpMessageConverter]]"
---
`RequestEntity`：封装请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参

可以通过`getHeaders()`获取请求头信息，通过`getBody()`获取请求体信息

后台测试代码

```java
@PostMapping("/testRequestEntity")
public String testRequestEntity(RequestEntity<String> requestEntity) {
    System.out.println("requestHeader: " + requestEntity.getHeaders());
    System.out.println("requestBody: " + requestEntity.getBody());
    return "success";
}
```

前台测试代码

```html
<form th:action="@{httpController/testRequestEntity}" method="post">
    用户名：<input type="text" name="username"><br/>
    密码：<input type="password" name="password"><br/>
    <input type="submit" value="测试RequestEntity">
</form>
```

测试结果

![[1755507292871_61cPGc3MxD.gif]]

后台日志信息

```shell
requestHeader: [host:"localhost:8080", connection:"keep-alive", content-length:"30", cache-control:"max-age=0", sec-ch-ua:"" Not A;Brand";v="99", "Chromium";v="99", "Google Chrome";v="99"", sec-ch-ua-mobile:"?0", sec-ch-ua-platform:""Windows"", upgrade-insecure-requests:"1", origin:"http://localhost:8080", user-agent:"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36", accept:"text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9", sec-fetch-site:"same-origin", sec-fetch-mode:"navigate", sec-fetch-user:"?1", sec-fetch-dest:"document", referer:"http://localhost:8080/SpringMVC/", accept-encoding:"gzip, deflate, br", accept-language:"zh-CN,zh;q=0.9", Content-Type:"application/x-www-form-urlencoded;charset=UTF-8"]
requestBody: username=admin&password=123456
```