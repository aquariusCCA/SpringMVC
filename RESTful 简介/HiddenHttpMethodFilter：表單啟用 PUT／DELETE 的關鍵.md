---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[RESTful 简介]]"
  - "[[使用 RESTful 模拟操作用户资源]]"
---
在 **@RequestMapping 注解** 中，我们提到过 [[form 表单支持哪些请求方式？|form 表单默认只支持GET 和 POST 请求，如果直接通过 method 属性指定为 PUT 和 DELETE，会默认以 GET 请求方式处理]]

而想要实现`PUT`和`DELETE`请求，需要在 `web.xml`中配置 SpringMVC 提供的`HiddenHttpMethodFilter` 过滤器，并在前台页面使用隐藏域来设置`PUT`和`DELETE`类型的请求方式

当然，也可以使用`AJAX`发送`PUT`和`DELETE`请求，但是需要注意`PUT`和`DELETE`仅部分浏览器支持

为了更清楚地了解`HiddenHttpMethodFilter`过滤器到底干了什么，这里对其源码进行剖析

```java
public class HiddenHttpMethodFilter extends OncePerRequestFilter {
    private static final List<String> ALLOWED_METHODS = Collections.unmodifiableList(Arrays.asList(HttpMethod.PUT.name(),HttpMethod.DELETE.name(),HttpMethod.PATCH.name()));
    public static final String DEFAULT_METHOD_PARAM = "_method";
    private String methodParam = DEFAULT_METHOD_PARAM;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
        throws ServletException, IOException {
        HttpServletRequest requestToUse = request;
        // 要求原请求方式为 POST 请求
        if ("POST".equals(request.getMethod()) && request.getAttribute(WebUtils.ERROR_EXCEPTION_ATTRIBUTE) == null) {
            // methodParam值：_method
            // paramValue值：_method属性值
            String paramValue = request.getParameter(this.methodParam);
            if (StringUtils.hasLength(paramValue)) {
                // _method属性值转为全大写形式，put==>PUT，delete==>DELETE
                String method = paramValue.toUpperCase(Locale.ENGLISH);
                // 判断_method属性值是否为{"PUT","DELETE","PATCH"}中的一个
                if (ALLOWED_METHODS.contains(method)) {
                    // “偷梁换柱”，包装为一个新的请求对象
                    requestToUse = new HttpMethodRequestWrapper(request, method);
                }
            }
        }
        filterChain.doFilter(requestToUse, response);
    }
    
    private static class HttpMethodRequestWrapper extends HttpServletRequestWrapper {
        private final String method;
        public HttpMethodRequestWrapper(HttpServletRequest request, String method) {
            super(request);
            this.method = method;
        }
        @Override
        public String getMethod() {
            return this.method;
        }
    }
}
```

“源码在手，天下我有”。接下来，将理论付诸实践

---

