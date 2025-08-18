---
up:
  - "[[Spring MVC 課程描述]]"
---
EmployeeController.java

```java
@GetMapping("/employee")
public String getAllEmployee(Model model) {
    List<Employee> employeeList = employeeDao.getAll();
    model.addAttribute("employeeList", employeeList);
    return "employeelist";
}
```

employeelist.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>员工信息</title>
        <style type="text/css">
            table {
                width: 50%;
                border: 1px black solid;
                border-collapse: collapse;
                vertical-align: middle;
                text-align: center;
            }
            tbody tr:nth-child(odd) {
                background-color: rgb(211, 216, 188);
            }
            th, td {
                border: 1px black solid;
            }
        </style>
    </head>
    <body>
        <table>
            <tr>
                <th colspan="5">员工信息</th>
            </tr>
            <tr>
                <th>ID</th>
                <th>姓名</th>
                <th>邮箱</th>
                <th>性别</th>
                <th>操作</th>
            </tr>
            <tr th:each="employee : ${employeeList}">
                <td th:text="${employee.id}"></td>
                <td th:text="${employee.lastName}"></td>
                <td th:text="${employee.email}"></td>
                <td th:text="${employee.gender == 1 ? '男' : '女'}"></td>
                <td>
                    <a href="">修改</a>
                    <a href="">删除</a>
                </td>
            </tr>
        </table>
    </body>
</html>
```

效果

![[1755503612826_QRjo8FtIg1.png]]