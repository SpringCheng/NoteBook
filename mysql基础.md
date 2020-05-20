**mysql基础**

**一、基础查询**

- 语法：select 查询列表 from 表名

- 查询常量

  ```mysql
  select 100 as number,ename,age from emp
  ```

- 查询表达式

  ```mysql
  select 100%98
  ```

- 查询函数

  ```
  select version()
  ```

- 起别名

  1. 使用as关键字

     ```mysql
     select 100%98 as "结果"
     select last_name as "姓",first_name as "名" from employees
     ```

  2. 使用空格

     ```mysql
     select last_name "姓",first_name "名" from employees
     ```

- 去重

  ```mysql
  select distinct department_id from employees
  ```

  

**二、条件查询**

1. **按条件表达式筛选**

   > 简答的条件表达式：> ,<, =, != ,<> ,>= ,<=

   1. 查询部门编号不等于90号的员工名和部门名

      ```mysql
      select last_name ,department_id from employees where department_id <>90
      ```

2. **按逻辑表达式筛选**

   > &&, || ,!

3. **模糊查询**

   - like (%任意多个字符，_任意单个字符)

     ```mysql
     select * from employees where last_name like '%a%'
     ```

   - between and

     ```mysql
     SELECT
         *
     FROM
         employees
     WHERE
         employee_id BETWEEN 100 AND 120;
     ```

   -  in

     ```mysql
     SELECT
         last_name,
         job_id
     FROM
         employees
     WHERE
         job_id IN( 'IT_PROT' ,'AD_VP','AD_PRES');
     ```

   - is null

     - 仅仅可以判断NULL值

**三、排序查询**

1. 语法：

   1. asc 代表升序，可以省略
   2. desc代表降序
   3. order by 可以支持单个字段，表达式，别名 ，函数多个字段
   4. order by子句在查询语句的最后面，除了limit子句

2. 案例

   1. 按单个字段排序

      ```mysql
      SELECT * FROM employees ORDER BY salary DESC;
      ```

   2. 添加筛选条件后再排序

      ```mysql
      select * from employees where department_id>=90 orderby employees_id DESC
      ```

   3. 按表达式排序

      ```mysql
      select *,salar*12*(1+ifnull(commission_pict,0)) from emp order by salar*12*(1+ifnull(commission_pict,0))
      ```

   4. 按别名排序

      ```mysql
      select *,salar*12*(1+ifnull(commission_pict,0)) as yearsal from emp order by  yearsal
      ```

   5. 按函数排序

      ```mysql
      select length(last_name),last_name from employess order by length(last_name) DESC;
      ```

   6. 按多个字段排序

      ```mysql
      查询员工信息，要求先按工资降序，再按employee_id升序
      select sal,employee_id from meployees order sal DESC,employee_id ASC
      ```

**四、常见函数**

- 单行函数

  1. lenngth

  2. concat

     ```mysql
     concat 拼接字符串
     SELECT CONCAT(last_name,'_',first_name) 姓名 FROM employees;
     ```

     

  3. substr，substring

     ```mysql
     注意：索引从1开始
     #截取从指定索引处后面所有字符
     SELECT SUBSTR('李莫愁爱上了陆展元',7)  out_put;
     #截取从指定索引处指定字符长度的字符
     SELECT SUBSTR('李莫愁爱上了陆展元',1,3) out_put;
     #案例：姓名中首字符大写，其他字符小写然后用_拼接，显示出来
     SELECT CONCAT(UPPER(SUBSTR(last_name,1,1)),'_',LOWER(SUBSTR(last_name,2)))  out_put
     FROM employees;
     ```

     

  4. instr

     ```mysql
     instr 返回子串第一次出现的索引（起始索引），如果找不到返回0
     SELECT INSTR('杨不殷六侠悔爱上了殷六侠','殷八侠') AS out_put;
     ```

     

  5. trim

     ```mysql
     trim 去掉前后空格或去掉前后指定字符
     SELECT LENGTH(TRIM('    张翠山    ')) AS out_put;
     SELECT TRIM('aa' FROM 'aaaaaaaaa张aaaaaaaaaaaa翠山aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa')  AS out_put;
     ```

     

  6. upper，lower

     ```mysql
     姓变大写，名变小写，然后拼接
     select concat(upper(last_name),lower(first_name)) 姓名 from employess
     ```

  7. lpad

     ```mysql
     lpad 用指定的字符实现左填充指定长度
     SELECT LPAD('殷素素',10,'*') AS out_put;
     ```

     

  8. rpad

     ```mysql
     rpad 用指定的字符实现右填充指定长度
     SELECT RPAD('殷素素',12,'ab') AS out_put;
     ```

     

  9. replace

     ```mysql
     SELECT REPLACE('周芷若周芷若周芷若周芷若张无忌爱上了周芷若','周芷若','赵敏') AS out_put;
     ```

     

- 数学函数

  1. round  四舍五入
  2. ceil    向上取整
  3. floor 向下取整
  4. truncate 截断 （直接截取不四舍五入）
  5. mod  取余

- 日期函数

  1. now
  2. curdate
  3. curtime
  4. year
  5. month
  6. day
  7. hour
  8. minute
  9. second
  10. str_to_date 
  11. date_fromat

- 其他函数

  1. versiong
  2. database
  3. user

- 控制函数

  1. if

  2. case

     ```mysql
     SELECT salary,
     CASE 
     WHEN salary>20000 THEN 'A'
     WHEN salary>15000 THEN 'B'
     WHEN salary>10000 THEN 'C'
     ELSE 'D'
     END AS 工资级别
     FROM employees;
     
     SELECT salary 原始工资,department_id,
     CASE department_id
     WHEN 30 THEN salary*1.1
     WHEN 40 THEN salary*1.2
     WHEN 50 THEN salary*1.3
     ELSE salary
     END AS 新工资
     FROM employees;
     ```

     

**五、分组函数**

1. 分类
   1. sum
   2. avg
   3. max
   4. min
   5. count
2. 特点
   1. sum，avg一般用于处理数据值型，max，min，count可以处理任何数据
   2. 以上分组函数都忽略null值，只计算不为null的数值
   3. 可以和distinct搭配实现去重的运算
   4. count函数的单独，一般使用count(*)用来统计行数
   5. 和分组函数一桶查询的字段要求是group by后的字段

**六、分组查询**

1. 语法：

   - ###### select 分组函数，字段（要求出现在group by的后面）from 表 【where筛选条件】group by分组字段 【order by排序的字段】

2. 特点：

   1. 和分组函数一同查询的字段必须是group by后面出现的字段

   2. 筛选分为两类，分组前你筛选和分组后筛选

      | 针对的表     | 位置               | 连接的关键词      |
      | ------------ | ------------------ | ----------------- |
      | 分组前的筛选 | 原始表             | group by 前 where |
      | 分组后的筛选 | group by后的结果集 | group by后 having |

   3. 分组可以按单个字段也可以按多个字段

   4. 可以搭配排序使用

3. 案例

   ```mysql
   1.简单的分组
   #案例1：查询每个工种的员工平均工资
   SELECT AVG(salary),job_id FROM employees GROUP BY job_id;
   
   #案例2：查询每个位置的部门个数
   select couont(*),location_id from departments group by location_id;
   ```

   ```mysql
   2、可以实现分组前的筛选
   #案例1：查询邮箱中包含a字符的 每个部门的最高工资
   select max(salry),department_id from employess where emial='%a%' group by department_id;
   
   #案例2：查询有奖金的每个领导手下员工的平均工资
   select avg(salary),manager_id from employees where commission_pct is not null group by manager_id;
   
   ```

   ```mysql
   3、分组后筛选
   #案例1：查询哪个部门的员工个数>5
   select deparment_id,count(*) from employees group by department_id having count(*)>5
   
   #案例2：每个工种有奖金的员工的最高工资>12000的工种编号和最高工资
   select department_id,max(salary) from empyloyess where commission is not null group by department_id having max(salary)>12000
   
   #案例3：领导编号>102的每个领导手下的最低工资大于5000的领导编号和最低工资
   select manager_id,min(salary) as minsalary from employees where manager_id>102 group by manager_id having minsalary>5000
   ```

   ```mysql
   4.添加排序
   #案例：每个工种有奖金的员工的最高工资>6000的工种编号和最高工资,按最高工资升序
   select job_id,max(salary) from empyloyees where commissin_pct is not null group by job_id having max(salary)>6000 order by ASC;
   ```

   ```mysql
   #5.按多个字段分组
   #案例：查询每个工种每个部门的最低工资,并按最低工资降序
   select job_id,department_id,min(salary) from employees group by department_id,job_id order by min(salary) DESC
   ```

   

**七、连接查询**

1. **分类**

   1. sql92标准：仅仅支持内连接
   2. sql99标准：支持内连接+外连接（左右外连接）+交叉联结
   3. 按功能划分：
      1. 内连接：
         1. 等值连接
         2. 非等值连接
         3. 自连接
      2. 外连接：
         1. 左外连接
         2. 右外连接
         3. 全外连接
      3. 交叉连接

2. 案例

   1. sql92标准内连接

      ```mysql
      #查询员工名和对应的部门名
      SELECT last_name,department_name
      FROM employees,departments
      WHERE employees.`department_id`=departments.`department_id`;
      ```

   2. 为表起别名

      ```mysql
      #查询员工名、工种号、工种名
      SELECT e.last_name,e.job_id,j.job_title
      FROM employees  e,jobs j
      WHERE e.`job_id`=j.`job_id`;
      ```

   3. 自连接

      ```mysql
      #案例：查询 员工名和上级的名称
      select e.employee_id,e.last_name,m.employee_id,m.last_name from employees e,employees m where e.manger_id=m.employee_id
      ```

3. **案例**

   1. 内连接

      1. 语法：select 查询列表 from 表1 别名 【jion】 表2 别名 on 连接条件【where 筛选条件】【group by 分组】【having 筛选条件】【order by排序列表】

      2. 等值连接

         ```mysql
         #查询哪个部门的员工个数>3的部门名和员工个数，并按个数降序
         1.查询每个部门的员工个数
         select count(*),department_name from employees e inner join depaerments d on e.department_id=d.depaertmen_id group by deparment_name;
         2.在1的基础上筛选员工个数>3，并排序
         select count(*),department_name from employees e inner join depatments d on e.department_id=d.department_id group by department_name having count(*)>3 order by DESC;
         ```

         

      3. 非等值连接

         ```mysql
         #查询工资级别的个数>20的个数，并且按工资级别降序
          select count(*),grade_level from employees e join job_grade j on e.salary between g.lowest_sal and g.hightest_sal group by grade_level having count(*)>20 order by grade_level DESC;
         ```

         

      4. 自连接

         ```mysql
         #查询姓名中包含字符k的员工的名字、上级的名字
         select e.last_name,m.last_name from employees e join employees m on e.manager_id=m.employee_id;
         ```

   2. 外连接

      1. 引用场景：用于查询一个表中有，另一个表没有的记录

      2. 特点：

         1. 外连接的查询结果为主表中的所有记录
         2. 如果从表中有和他匹配的，则显示匹配的值
         3. 如果从表中没有和他匹配的，则显示null
         4. 外连接查询结果=内连接结果+主表中有而从表中没有的记录
         5. 左外连接，left join左边是主表
         6. 右外连接，right join右边是主表

      3. 案例

         - 左外连接

           ```mysql
           select b.*,bo.* from boys bo left join beauty b on b.boyfriend_id=bo.id where b.id is null
           ```

         

         - 右外连接

           ```mysql
           select d.*,e.employee_id from employee e right join departments d on d.department_id=e.department_id where e.employee_id is null
           ```

         - 全外连接

           ```mysql
           select b.*,bo.* from beauty b full join boys b on b.boyfriend_id=b.id
           ```

      4. 题目

         ```mysql
         #一、查询编号>3的女神的男朋友信息，如果有则列出详细，如果没有，用null填充
         SELECT b.id,b.name,bo.*
         FROM beauty b
         LEFT OUTER JOIN boys bo
         ON b.`boyfriend_id` = bo.`id`
         WHERE b.`id`>3;
         #二、查询哪个城市没有部门
         SELECT city
         FROM departments d
         RIGHT OUTER JOIN locations l 
         ON d.`location_id`=l.`location_id`
         WHERE  d.`department_id` IS NULL;
         #三、查询部门名为SAL或IT的员工信息
         SELECT e.*,d.department_name,d.`department_id`
         FROM departments  d
         LEFT JOIN employees e
         ON d.`department_id` = e.`department_id`
         WHERE d.`department_name` IN('SAL','IT');
         
         SELECT * FROM departments
         WHERE `department_name` IN('SAL','IT');
         ```

         

**八、子查询**

1. 含义：出现在其他语句中的select语句，称为子查询或内查询

2. 分类：

   > select 后面：
   > 	**仅仅**支持标量子查询
   > from 后面：
   > 	支持表子查询
   > where 或having后面：
   > 	标量子查询（单行）
   > 	列子查询（多行）
   > 	行子查询
   > exists 后面：
   > 	表子查询

   > 标量子查询（结果集只有一行一列） 配合 > < <= >= <>
   > 列子查询（结果集只有一列多行）   配合 in any/some all
   > 行子查询（结果集一行多列）
   > 表子查询（结果集一般为多行多列）

**一、where或者having后面**

1. 标量子查询

   ```mysql
   #案例1：谁的工资比 Abel 高?
   select * from employees where salary>(select salary from employees where last_name='Able')
   
   #案例2：job_id与141号员工相同，salary比143号员工多的员工 姓名，job_id 和工资
   select last_name,job_id,salary from employees where 
   job_id=(select job_id from employees where job_id=141) 
   and salary>(select salary from employees where employees_id=141)
   
   #案例3：公司工资最少的员工的last_name,job_id和salary
   select last_name,job_id,salary from employees where salary=(select min(salary) from employees)
   
   #案例4：查询最低工资大于50号部门最低工资的部门id和其最低工资
   select department_id,min(salary) from employees group by department_id having min(salary)>(select salary from employees where department_id=50)
   
   ```

2. 列子查询（一列多行）

   ```mysql
   #案例1：location_id是1400或1700的部门中的所有员工姓名
   select last_name from employees where department_id <>all(select depaertment_id from departments wehre location_id in(1400,1700))
   
   #案例2：其它工种中比job_id为‘IT_PROG’工种任一工资低的员工的员工号、姓名、job_id 以及salary
   select last_name,job_id,salary from employees where 
   salary<any(
   select distinct salary from employees where job_id='IT_PROG')
   
   #案例3：返回其它部门中比job_id为‘IT_PROG’部门所有工资都低的员工的员工号、姓名、job_id 以及salary
   select job_id,last_name,salary from employees salary<all(select salary from employees where job_id='IT_PROG')
   ```

3. 行子查询（一行多列或多行多列）

   ```mysql
   #案例：查询员工编号最小并且工资最高的员工信息
   1、查询员工编号最小的员工信息
   select min(employees_id) from employees
   2、查询工资最高的员工信息
   select max(salary) from employees
   3、查询员工信息
   select * from employees where employees_id=
   (select min(employees_id) from employees) 
   and salary=
   (select max(salary) from employees)
   ```

**二、select后面**（仅仅支持标量子查询（>,<,=>,>=,<>）,查询结果集为一行一列）

1. ```mysql
   #案例1：查询每个部门的员工个数
   select d.*,(select couont(*) from employees e where e.department_id=d.department_id) 个数 from department d;
   ```

**三、from后面**（**将子查询结果充当一张表，要求必须起别名**）

1. ```mysql
   # 案例1：查询每个部门的平均工资的工资等级
   1、查询每个部门的平均工资
   select avg(salary),department_id from meployees group by department_id 
   2、合并
   select ag_dep.*,g.grade_level from 
   (select avg(salary),department_id from meployees group by department_id ) ag_dep
   inner join job_grades g 
   on ag_dep.ag betweent lowest_sal and highest_sal;
   ```

**四、exists后面**

1. 语法：exists（完整的查询语句）

2. 结果：1或0

   ```mysql
   select exists(select employee_id from employees where salary=3000)
   
   #案例1：查询有员工的部门名
   方法1：
   select department_name from departments d where d.department_id in(select department_id from meployees)
   方法2：
   select department_name from departments d where exists(
   select * from employees e where d.department_id=e.deparment_id)
   ```

**题目：**

```mysql
#1. 查询和Zlotkey相同部门的员工姓名和工资
#①查询Zlotkey的部门
SELECT department_id
FROM employees
WHERE last_name = 'Zlotkey'
#②查询部门号=①的姓名和工资
SELECT last_name,salary
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM employees
    WHERE last_name = 'Zlotkey'
)

#2.查询工资比公司平均工资高的员工的员工号，姓名和工资。
#①查询平均工资
SELECT AVG(salary)
FROM employees
#②查询工资>①的员工号，姓名和工资。
SELECT last_name,employee_id,salary
FROM employees
WHERE salary>(
    SELECT AVG(salary)
    FROM employees
);


#3.查询各部门中工资比本部门平均工资高的员工的员工号, 姓名和工资
#①查询各部门的平均工资
SELECT AVG(salary),department_id
FROM employees
GROUP BY department_id
#②连接①结果集和employees表，进行筛选
SELECT employee_id,last_name,salary,e.department_id
FROM employees e
INNER JOIN (
    SELECT AVG(salary) ag,department_id
    FROM employees
    GROUP BY department_id

) ag_dep
ON e.department_id = ag_dep.department_id
WHERE salary>ag_dep.ag ;


#4. 查询和姓名中包含字母u的员工在相同部门的员工的员工号和姓名
#①查询姓名中包含字母u的员工的部门
SELECT  DISTINCT department_id
FROM employees
WHERE last_name LIKE '%u%'
#②查询部门号=①中的任意一个的员工号和姓名
SELECT last_name,employee_id
FROM employees
WHERE department_id IN(
    SELECT  DISTINCT department_id
    FROM employees
    WHERE last_name LIKE '%u%'
);

#5. 查询在部门的location_id为1700的部门工作的员工的员工号
#①查询location_id为1700的部门
SELECT DISTINCT department_id
FROM departments 
WHERE location_id  = 1700

#②查询部门号=①中的任意一个的员工号
SELECT employee_id
FROM employees
WHERE department_id =ANY(
    SELECT DISTINCT department_id
    FROM departments 
    WHERE location_id  = 1700
);
#6.查询管理者是King的员工姓名和工资
#①查询姓名为king的员工编号
SELECT employee_id
FROM employees
WHERE last_name  = 'K_ing'
#②查询哪个员工的manager_id = ①
SELECT last_name,salary
FROM employees
WHERE manager_id IN(
    SELECT employee_id
    FROM employees
    WHERE last_name  = 'K_ing'
);
#7.查询工资最高的员工的姓名，要求first_name和last_name显示为一列，列名为 姓.名

#①查询最高工资
SELECT MAX(salary)
FROM employees
#②查询工资=①的姓.名
SELECT CONCAT(first_name,last_name) "姓.名"
FROM employees
WHERE salary=(
    SELECT MAX(salary)
    FROM employees
);


```





**九、分页查询**

- 应用场景：当要显示的数据，一页显示不全，需要分页提交sql请求

- 语法：

  ```mysql
  select 查询列表
  from 表
  【join type join 表2
  on 连接条件
  where 筛选条件
  having 分组后的筛选
  order by排序的字段】
  limit【offset，】size
  
  offset：要显示条目的起始索引（起始索引从0开始）
  size：要显示的条目个数
  ```

- 特点：

  1. limit语句要放在查询语句的最后

**十、联合查询**

1. 应用场景：
   - 要查询的结果来自多个表，且多个表没有直接的连接关系，但查询的信息一致时
2. 特点
   - 要求多条查询语句的查询列数是一致的
   - 要求多条查询语句的查询的每一列的类型个顺序最好一致
   - union关键词默认去重，如果使用union all可以包含重复项

**十一、DML语言**

1. **insert**

   ```mysql
   语法：insert into 表名（列名,...)values（值1，...);
   
   - 支持多行插入
   ```

   ```mysql
   语法2：inset into 表名 set 列名=值，列名=值
   
   - 不支持多行插入
   ```

2. **update**

   - 语法：

     ```mysql
     update 表名 set 列=新值，列=新值  where 筛选条件；
     ```

3. **delete**

   - 语法：

     ```mysql
     delete from 表名 where 筛选条件
     
     truncat table 表名
     ```

**十二、DDL语言**

1. **create**

   1. 库

      ```mysql
      create database 【if not exists】 库名
      ```

   2. 表

      ```mysql
      create table tb_name(
      
      列名 列的类型 约束，
      
      列名 列的类型 约束，
      
      列名 列的类型 约束
      
      。。。
      
      )
      ```

      

2. **alter**

   1. 库：

      ```mysql
      - rename database 旧库名 to 新库名
      
      - alter database 库名 character set gbk
      ```

      

   2. 表：

      ```mysql
      语法：alter table tb_name add|drop|modify|change column 列名 【列类型，约束】
      ```

      1. 修改列名：

         ```mysql
         alter table book change column publishdate pubDate DATETIME;
         ```

      2. 修改列的类型或约束：

         ```mysql
         alter table book modify column pubdate timestamp
         ```

      3. 添加新列

         ```mysql
         alter table book add column annual double;
         ```

      4. 删除列

         ```mysql
         alter table book drop column annual
         ```

      5. 修改表名

         ```mysql
         alter table book author rename book_author
         ```

3. **drop**

   1. 库：

      ```mysql
      drop database if exists 库名
      ```

   2. 表：

      ```mysql 
      drop table if exists book_author;
      ```

**十三、约束**

1. not null
2. primary key
3. default
4. unique
5. check
6. foreign key
7. auto_increment
   - set auto_crement=3设置步长为3

**十四、事务：**

- **特征**
  - 原子性：最小单位，不可再分
  - 一致性：所有DML语言操作的时候，必须保证同时成功或者同时失败
  - 隔离性：一个事务不会影响到别的事务的运行
  - 持久性：在事务完成之后，钙食物对数据库所做的更改将持久第保存在数据库中，并不会回滚
- **步骤**
  1. 开启事务：start transaction
  2. 结束事务：end transaction
  3. 提交事务：commit transaction
  4. 回滚事务：rollback transaction
- **其他**
  - 开启事务的标志：任何一条DML语句的执行，标志事务的开始
  - 结束事务的标志：提交或回滚
    - 提交：将所有的DML语句操作记录在底层硬盘文件中数据进行一次同步；
    - 回滚：失败的结束，将所有的DML语句操作记录全部清空
  - 在事务进行过程中，未结束之前，DML都不会修改底层数据库文件中的数据，只是将历史操作记录一下，在内存中完成记录，只有在事务成功的结束时，才会是修改底层硬盘文件中的数据。
  - mysql数据库管理系统中，默认情况下，事务是自动提交的，也就是说，只执行一条DML语句，就开启了事务，并且提交了事务。

- **事务的提交和回滚实例**
  - **关闭mysql事务自动提交**
    - 成功用法：
      - start transaction;开启事务
      - DML语句 执行
      - commit 手动提交事务
    - 失败用法：
      - start transaction;开启事务
      - DML语句 执行
      - rollback 
      - commit 手动提交事务
  - **关闭和开启自动提交事务(只对当前窗口有效)**
    - 关闭自动提交事务
      - set autocommit=off
      - set session autocommit=off
    - 打开自动提交事务
      - set autocommit=on
      - set session autocommit=on
  - **查询事务状态**
    - show variables like '%commit%';







**十五、索引**

- 添加索引

  - 能够通过查询的尽量通过主键查询，效率较高
  - 索引和表相同都是存储在硬盘改文件中的
  - 索引和表相同，都是一个对象，表是存储在硬盘文件中的

- **检索方式**

  1. 全表扫描（效率较低）：

     ###### 举例：

     - 查询ename=‘King’
     - select * from emp where ename='King'
       - 若没有添加索引，name通过ename过滤数据的时候，ename字段会全表扫描

  2. 通过索引扫描（提高查询效率）

     - create index  searname on emp(ename)   //创建索引
     - 



- 索引的引用

  1. 创建索引

     1. 直接创建

        ```
        - create index 索引名 on 表名 （列名）
        - 或 create unique  index 索引名 on 表名（列名）  //添加unique表示在该列添加一个唯一性索引
        ```

     - 修改表结构（创建索引）

       ```
       alter table tb_name add index index_name(columnName)
       ```

     - 创建表的时候直接指定

       ```
       create table tb_name{
       id int NULL,
       username varchar(16) NOT NULL,
       index [index_name](username(length))
       };
       ```

  2. 查看索引

     - show index from dept

  3. 使用索引

     - 

  4. 删除索引

     - drop index 索引名 on 表名

     ```
     drop index searname on emp
     ```

     



**十六、视图**

1. **什么是视图**

   - 视图在数据库系统中也是一个对象，也是以文件形式存在的，视图也对应了一个查询结果，只是从不同的角度查看数据

2. **创建视图**

   1. 语法结构：create view v_name as 查询语句；
   2. 例子：
      1. 将查询结果作为一个视图创建出来：create view myview as select ename,sal from emp;
      2. 通过视图对象看数据：select * from myview
      3. 注意：
         1. 视图中的数据是脱离对象的
         2. 视图中的数据也可以进行增删改，但是视图中的增删改与原表无关
         3. 只能将查询结果作为视图创建出来

3. **删除视图**

   - drop view if exists v_name

4. **修改视图**

   - alter view v_name as 查询语句
   - 例子：
     - alter veiw myview as select ename,sal,hiredate from emp;

5. **视图的作用**

   1. 面向视图查询，可以提高效率
   2. 隐藏表的实现细节（重点）

6. **创建视图注意事项**

   - 创建视图的select语句中不能有子查询