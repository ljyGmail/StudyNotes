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
