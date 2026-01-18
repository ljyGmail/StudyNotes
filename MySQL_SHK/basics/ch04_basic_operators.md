# 第 4 章 运算符

> 18 算术运算符的使用

## 1. 算术运算符

```mysql
# 1. 加法与减法运算符
SELECT 100,
       100 + 0,
       100 - 0,
       100 + 50,
       100 + 50 * 30,
       100 + 35.5,
       100 - 35.5
FROM DUAL;
# 在SQL中，+没有连接字符串的作用，只表示加法运算。此时，会将字符串转换为数值(隐式转换)。
SELECT 100 + '1' # 在Java语言中，结果是: 1001。
FROM DUAL; # 101

SELECT 100 + 'a' # 此时将'a'看作0来处理。
FROM DUAL; # 100

SELECT 100 + NULL # NULL值参与运算，结果为NULL。
FROM DUAL;
# NULL

# 2. 乘法与除法运算符
SELECT 100,
       100 * 1,
       100 * 1.0,
       100 / 1.0,
       100 / 2,
       100 + 2 * 5 / 2,
       100 / 3,
       100 DIV 0 # 分母如果为0，则结果为NULL。
FROM DUAL;

# 3. 求模(求余)运算符
SELECT 12 % 3, 12 MOD 5, 12 MOD -5, -12 % 5, -12 % -5
FROM DUAL;

# 练习: 查询员工ID为偶数的员工信息。
SELECT employee_id, last_name, salary
FROM employees
WHERE employee_id MOD 2 = 0;
```

> 19 比较运算符的使用

## 2. 比较运算符

```mysql
# 1. 等号运算符 =
SELECT 1 = 1,
       1 = '1',
       1 = 0,
       'a' = 'a',
       (5 + 3) = (2 + 6),
       '' = NULL,
       NULL = NULL;

SELECT 1 = 2, 1 != 2, 1 = '1', 1 = 'a', 0 = 'a' # 字符串存在隐式转换。如果转换数值不成功，则看作是0。
FROM DUAL;

SELECT 'a' = 'a', 'ab' = 'ab', 'a' = 'b' # 两边都是字符串的话，则按照ANSI的比较规则进行比较。
FROM DUAL;

SELECT 1 = NULL, NULL = NULL # 只要有NULL参与判断，结果就为NULL。
FROM DUAL;

SELECT last_name, salary, commission_pct
FROM employees
# WHERE salary = 6000;
WHERE commission_pct = NULL;
# 此时执行，不会有任何结果。

# 2. 安全等于运算符 <=> 记忆技巧: 为NULL而生。
/*
安全等于运算符（<=>）与等于运算符（=）的作用是相似的， 唯一的区别 是‘<=>’可
以用来对NULL进行判断。在两个操作数均为NULL时，其返回值为1，而不为NULL；当一个操作数为NULL
时，其返回值为0，而不为NULL。
 */
SELECT 1 <=> '1',
       1 <=> NULL,
       'a' <=> 'a',
       (5 + 3) <=> (2 + 6),
       '' <=> NULL,
       NULL <=> NULL
FROM DUAL;

SELECT 1 <=> 2, 1 <=> '1', 1 <=> 'a', 0 <=> 'a'
FROM DUAL;

SELECT 1 <=> NULL, NULL <=> NULL
FROM DUAL;

# 练习: 查询表中commission_pct为NULL的数据有哪些。
SELECT last_name, salary, commission_pct
FROM employees
WHERE commission_pct <=> NULL;

# 3. 不等于运算符 (<>或!=)
SELECT 1 <> 1,
       1 != 2,
       'a' != 'b',
       (3 + 4) <> (2 + 6),
       'a' != NULL,
       NULL <> NULL;

SELECT 3 <> 2, '4' <> NULL, '' != NULL, NULL != NULL

# 4. 空运算符 (IS NULL或ISNULL函数)
SELECT NULL IS NULL, ISNULL(NULL), ISNULL('a'), 1 IS NULL
FROM DUAL;

# 练习: 查询表中commission_pct为NULL的数据有哪些。
SELECT last_name, salary, commission_pct
FROM employees
WHERE commission_pct IS NULL;
# 或
SELECT last_name, salary, commission_pct
FROM employees
WHERE ISNULL(commission_pct);

# 5. 非空运算符 (IS NOT NULL)

# 练习: 查询表中commission_pct不为NULL的数据有哪些。
SELECT last_name, salary, commission_pct
FROM employees
WHERE commission_pct IS NOT NULL;
# 或
SELECT last_name, salary, commission_pct
FROM employees
WHERE NOT commission_pct <=> NULL;

# 6. 最小值运算符 (LEAST(值1，值2，...，值n))
SELECT LEAST(1, 0, 2), LEAST('b', 'a', 'c'), LEAST(1, NULL, 2);

SELECT LEAST(first_name, last_name), LEAST(LENGTH(first_name), LENGTH(last_name))
FROM employees;

# 7. 最大值运算符 (GREATEST(值1，值2，...，值n))
SELECT GREATEST(1, 0, 2), GREATEST('b', 'a', 'c'), GREATEST(1, NULL, 2);

# 8. BETWEEN AND运算符   BETWEEN 条件下界1 AND 条件上界2 (查询条件1和条件2范围内的数据，包含边界)
SELECT 1 BETWEEN 0 AND 1,
       10 BETWEEN 11 AND 12,
       'b' BETWEEN 'a' AND 'c';

# 查询工资在6000到8000的员工信息。
SELECT employee_id, last_name, salary
FROM employees
# WHERE salary BETWEEN 6000 AND 8000;
WHERE salary >= 6000
          && salary <= 8000;

# 交换6000和8000之后，查询不到数据。
SELECT employee_id, last_name, salary
FROM employees
WHERE salary BETWEEN 8000 AND 6000;

# 查询工资不在6000到8000的员工信息。
SELECT employee_id, last_name, salary
FROM employees
WHERE salary NOT BETWEEN 8000 AND 6000;
# WHERE salary < 6000
#    OR salary > 8000;

# 9. IN运算符 
SELECT 'a' IN ('a', 'b', 'c'), 1 IN (2, 3), NULL IN ('a', 'b'), 'a' IN ('a', NULL)
FROM DUAL;

# 练习: 查询部门为10, 20, 30部门的员工信息。
SELECT last_name, salary, department_id
FROM employees
# WHERE department_id = 10
#    OR department_id = 20
#    OR department_id = 30;
WHERE department_id IN (10, 20, 30);

# 10. NOT IN运算符 
SELECT 'a' NOT IN ('a', 'b', 'c'), 1 NOT IN (2, 3);

# 练习: 查询工资不是6000，7000，8000的员工信息。
SELECT last_name, salary, department_id
FROM employees
WHERE salary NOT IN (6000, 7000, 8000);

# 11. LIKE运算符  模糊查询
# %: 代表不确定个数的字符   _: 代表一个不确定的字符
SELECT NULL LIKE 'abc',
       'abc' LIKE NULL;
# 练习: 查询last_name中包含字符'a'的员工信息。
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%';

# 练习: 查询last_name中以字符'a'开头的员工信息。
SELECT last_name
FROM employees
WHERE last_name LIKE 'a%';

# 练习: 查询last_name中包含字符'a'且包含字符'e'的员工信息。
# 写法1:
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%'
  AND last_name LIKE '%e%';

# 写法2:
SELECT last_name
FROM employees
WHERE last_name LIKE '%a%e%'
   OR last_name LIKE '%e%a%';

# 练习: 查询last_name的第3个字符是'a'的员工信息。
SELECT last_name
FROM employees
WHERE last_name LIKE '__a%';

# 练习: 查询last_name的第2个字符是'_'且第3个字符是'a'的员工信息。
# 需要使用转义字符: \
SELECT *
FROM employees
WHERE last_name LIKE '_\_a%';
# 或者
SELECT *
FROM employees
WHERE last_name LIKE '_$_a%' ESCAPE '$';

# 12. REGEXP运算符  语法格式为： expr REGEXP 匹配条件
SELECT 'shkstart' REGEXP '^s',
       'shkstart' REGEXP 't$',
       'shkstart' REGEXP 'hk';

SELECT 'atguigu' REGEXP 'gu.gu',
       'atguigu' REGEXP '[ab]';
```

> 20 逻辑运算符与位运算符的使用

```mysql
# 1. 逻辑非运算符 (NOT或!)
SELECT NOT 1, NOT 0, NOT (1 + 1), NOT !1, NOT NULL;

SELECT last_name, job_id
FROM employees
WHERE job_id NOT IN ('IT_PROG', 'ST_CLERK', 'SA_REP');

# 2. 逻辑与运算符 (AND或&&)
SELECT 1 AND -1, 0 AND 1, 0 AND NULL, 1 AND NULL;

SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE salary >= 10000
  AND job_id LIKE '%MAN%';

# 3. 逻辑或运算符 (OR或||)
SELECT 1 OR -1, 1 OR 0, 1 OR NULL, 0 || NULL, NULL || NULL;

#查询基本薪资不在9000-12000之间的员工编号和基本薪资
SELECT employee_id, salary
FROM employees
WHERE NOT (salary >= 9000 AND salary <= 12000);

SELECT employee_id, salary
FROM employees
WHERE salary < 9000
   OR salary > 12000;

SELECT employee_id, salary
FROM employees
WHERE salary NOT BETWEEN 9000 AND 12000;

# ---
SELECT employee_id, last_name, job_id, salary
FROM employees
WHERE salary >= 10000
   OR job_id LIKE '%MAN%';

# 4. 逻辑异或运算符 (XOR)
SELECT 1 XOR -1, 1 XOR 0, 0 XOR 0, 1 XOR NULL, 1 XOR 1 XOR 1, 0 XOR 0 XOR 0;

SELECT last_name, department_id, salary
FROM employees
WHERE department_id IN (10, 20) XOR salary > 8000;
```

## 4. 位运算符

```mysql
# 1．按位与运算符
SELECT 1 & 10, 20 & 30;

# 2. 按位或运算符
SELECT 1 | 10, 20 | 30;

# 3. 按位异或运算符
SELECT 1 ^ 10, 20 ^ 30;

SELECT 12 & 5, 12 | 5, 12 ^ 5
FROM DUAL;

# 4. 按位取反运算符 
SELECT 10 & ~1;

# 5. 按位右移运算符
SELECT 1 >> 2, 4 >> 2;

# 6. 按位左移运算符
SELECT 1 << 2, 4 << 2;

# 在一定范围内满足: 每向左移动1位，相当于乘以2; 每向右移动一位，相当于除以2。
```

## 5. 运算符的优先级

- 只要记住当遇到不确定优先级的时候，只要加上括号就行了。

> 21 第4章运算符课后练习

## 课后练习

```mysql
# 1.选择工资不在5000到12000的员工的姓名和工资
SELECT last_name, salary
FROM employees
# WHERE salary NOT BETWEEN 5000 AND 12000;
WHERE salary < 5000
   OR salary > 12000;

# 2.选择在20或50号部门工作的员工姓名和部门号
SELECT last_name, department_id
FROM employees
# WHERE department_id IN (20, 50);
WHERE department_id = 20
   OR department_id = 50;

# 3.选择公司中没有管理者的员工姓名及job_id
SELECT last_name, job_id, manager_id
FROM employees
# WHERE manager_id IS NULL;
WHERE manager_id <=> NULL;

# 4.选择公司中有奖金的员工姓名，工资和奖金级别
DESC employees;
SELECT last_name, salary, commission_pct
FROM employees
# WHERE commission_pct IS NOT NULL;
WHERE NOT commission_pct <=> NULL;

# 5.选择员工姓名的第三个字母是a的员工姓名
SELECT last_name
FROM employees
WHERE last_name LIKE '__a%';

# 6.选择姓名中有字母a和k的员工姓名
SELECT last_name, salary
FROM employees
WHERE last_name LIKE '%a%k%'
   OR last_name LIKE '%k%a%';

SELECT last_name, salary
FROM employees
WHERE last_name LIKE '%a%'
  AND last_name LIKE '%k%';

# 7.显示出表 employees 表中 first_name 以 'e'结尾的员工信息
SELECT first_name, last_name, salary
FROM employees
# WHERE first_name LIKE '%e';
WHERE first_name REGEXP 'e$';
# 以'e'开头的写法: '^e'

# 8.显示出表 employees 部门编号在 80-100 之间的姓名、工种
SELECT last_name, job_id, department_id
FROM employees
# 方式1: 推荐
# WHERE department_id BETWEEN 80 AND 100;
# 方式2: 与方式1相同
# WHERE department_id >= 80
#   AND department_id <= 100;
# 方式3: 仅适用于本题
WHERE department_id IN (80, 90, 100);

SELECT *
FROM departments;

# 9.显示出表 employees 的 manager_id 是 100,101,110 的员工姓名、工资、管理者id
SELECT last_name, salary, manager_id
FROM employees
WHERE manager_id IN (100, 101, 110);
```
