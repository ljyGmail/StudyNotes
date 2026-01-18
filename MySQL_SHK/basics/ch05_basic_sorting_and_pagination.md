# 第 5 章 排序与分页

> 22 `ORDER BY`实现排序操作

## 1. 排序数据

```mysql
SELECT *
FROM employees;

# 如果没有使用排序操作，默认情况下查询返回的数据是按照添加数据的顺序显示的。

# 1. 基本使用
# 使用ORDER BY对查询到的数据进行排序操作。
# 升序: ASC (ascend)
# 降序: DESC (descend)

# 练习: 按照salary从高到低的顺序显示员工信息。
SELECT employee_id, last_name, salary
FROM employees
ORDER BY salary DESC;

# 练习: 按照salary从高到低的顺序显示员工信息。
SELECT employee_id, last_name, salary
FROM employees
ORDER BY salary ASC;

SELECT employee_id, last_name, salary
FROM employees
ORDER BY salary;
# 如果在ORDER BY后面没有显式指定排序方式的话，则默认按照升序排列。

# 我们可以使用列的别名进行排序。
SELECT employee_id, salary, salary * 12 annual_sal
FROM employees
ORDER BY annual_sal;

# 2. 列的别名只能在ORDER BY中使用，不能在WHERE中使用。
# 如下操作报错!
# SELECT employee_id, salary, salary * 12 annual_sal
# FROM employees
# WHERE annual_sal > 81600;

# 3. 强调格式: WHERE需要声明在FROM之后，ORDER BY之前。
SELECT employee_id, salary
FROM employees
WHERE department_id IN (50, 60, 70)
ORDER BY department_id DESC;

# 4. 二级排序

# 练习: 显示员工信息，按照department_id的降序排列，salary的升序排列。
SELECT employee_id, salary, department_id
FROM employees
ORDER BY department_id DESC, salary ASC;
```

