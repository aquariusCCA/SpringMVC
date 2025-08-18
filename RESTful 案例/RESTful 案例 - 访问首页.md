---
up:
  - "[[Spring MVC 課程描述]]"
---
index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
<h1>首页</h1>
<a th:href="@{/employeeController/employee}">查看员工信息</a>
</body>
</html>
```

测试

![[1755503612819_oih2AFXVOV.png]]