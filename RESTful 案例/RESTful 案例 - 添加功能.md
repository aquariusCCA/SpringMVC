---
up:
  - "[[Spring MVC 課程描述]]"
---
employeelist.html

```html
<!-- 略 -->
<tr>
    <th>ID</th>
    <th>姓名</th>
    <th>邮箱</th>
    <th>性别</th>
    <th>操作 <a th:href="@{/employeeController/toAdd}">添加</a><br/></th>
</tr>
<!-- 略 -->
```

employeeadd.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>添加员工</title>
    </head>
    <body>
        <h1>添加员工</h1>
        <form th:action="@{/employeeController/employee}" method="post">
            姓名：<input type="text" name="lastName"/><br/>
            邮箱：<input type="text" name="email"/><br/>
            性别：<input type="radio" name="gender" value="1">男
            <input type="radio" name="gender" value="0">女<br/>
            <input type="submit" value="添加">
        </form>
    </body>
</html>
```

EmployeeController.java

```java
@GetMapping("/toAdd")
public String toAdd() {
    return "employeeadd";
}

@PostMapping("/employee")
public String addEmployee(Employee employee) {
    employeeDao.save(employee);
    return "redirect:/employeeController/employee";
}
```

效果

![[1755503612826_SkPyM75DfO.gif]]