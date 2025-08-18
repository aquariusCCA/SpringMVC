---
up:
  - "[[Spring MVC 課程描述]]"
---
employeelist.html

```html
<!-- 略 -->
<a th:href="@{/employeeController/employee/}+${employee.id}">修改</a>
<!-- 略 -->
```

employeeedit.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>修改员工</title>
    </head>
    <body>
        <h1>修改员工</h1>
        <form th:action="@{/employeeController/employee}" method="post">
            <input type="hidden" name="_method" th:value="put">
            <input type="hidden" name="id" th:value="${employee.id}">
            姓名：<input type="text" name="lastName" th:value="${employee.lastName}"/><br/>
            邮箱：<input type="text" name="email" th:value="${employee.email}"/><br/>
            性别：<input type="radio" name="gender" value="1" th:field="${employee.gender}">男
            <input type="radio" name="gender" value="0" th:field="${employee.gender}">女<br/>
            <input type="submit" value="修改">
        </form>
    </body>
</html>
```

EmployeeController.java

```java
@GetMapping("/employee/{id}")
public String getEmployeeById(@PathVariable("id") Integer id, Model model) {
    Employee employee = employeeDao.getById(id);
    model.addAttribute("employee", employee);
    return "employeeedit";
}

@PutMapping("/employee")
public String editEmployee(Employee employee) {
    employeeDao.save(employee);
    return "redirect:/employeeController/employee";
}
```

效果

![[1755503612826_kOxarWugWc.gif]]