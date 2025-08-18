---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[HttpMessageConverter]]"
  - "[[@ResponseBody]]"
---
# 后台测试代码

```java
@PostMapping("/testAxios")
@ResponseBody
public String testAxios(User user) {
    return user.getUsername() + "," + user.getPassword();
}
```

---

# 前台测试代码

httpmessageconverter.html

```html
<div id="app">
    <a @click="testAxios" th:href="@{/httpController/testAxios}">SpringMVC 操作 AJAX</a>
</div>
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/static/js/axios.min.js}"></script>
<script type="text/javascript" th:src="@{/static/js/httpmessageconverter.js}"></script>
```

httpmessageconverter.js

```js
var vue = new Vue({
    el: "#app",
    methods: {
        testAxios: function (event) {
            testAxios(event.target.href);
        }
    }
});

function testAxios(url) {
    axios({
        method: "post",
        url: url,
        params: {
            username: "admin",
            password: "123456"
        }
    }).then(function (response) {
        alert(response.data);
    });
    event.preventDefault();
}
```

---

# 测试效果

![[1755508001254_IEvNfsbwgc.gif]]