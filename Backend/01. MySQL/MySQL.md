###  数据库的好处

1. 可以持久化数据到本地
2. 结构化查询

### 数据库的常见概念

1. DB：数据库，存储数据的容器
2. DBMS：数据库管理系统，又称为数据库软件或者数据库产品，用于创建或管理DB
3. SQL：结构化查询语言，用于和数据库通信的语言，不是某个数据库特有的，而是几乎所有主流数据库软件通用的语言

### 数据库存储数据的特点

1. 数据库存储在表中，然后表再放到库中
2. 一个库中可以有多个表，每张表用唯一的表名来标识自己
3. 表中有一个或多个列，列又称为“字段”，相当于java中的属性
4. 表中的一行数据相当于java中的一个对象

### MySQL背景

1. 前身属于瑞典的一家公司，MySQL AB
2. 08年被SUN公司收购
3. 09年SUN公司被Oracle收购

### MySQL的优点

1. 开源、免费、成本低
2. 性能高、移植性好
3. 体积小、便于安装

### MySQL常见命令

```sql
1. 查看当前所有的数据库
show databases;
show schemas;
2. 打开指定库
use DATABASE_NAME
3. 查看当前库的所有表
show tables;
4. 查看其它库的所有表
show tables from TABLE_NAME;
5. 创建表
create table TABLE_NAME(
  
  COLUMN_NAME TYPE,
  COLUMN_NAME TYPE,

)
6. 查看表结构
desc TABLE_NAME
7. 查看服务器版本
在mysql 里面
mysql select();
```

### MySQL语法规范

1. 不区分大小写，但是建议，关键字大写，表名，列名小写
2. 每条命令用`;`结尾
3. 每条命令根据需要，可以进行锁进或换行，建议关键字换行
4. 注释
   单行注释：`#注释文字`
   单行注释：`-- 注释文字`
   多行注释：`/* 注释文字 */`
5. 字符型和日期型常量必须用单引号印起来，数值型不需要

### DQL (Data Query Language)

```sql
#进阶1: 基础查询
/*
select 查询列表 from 表名

1. 查询列表可以是：表中的字段、常量值、表达式、函数
2. 查询的结果只是一个虚拟的表格
*/
```

起别名:
方式一：`select last_name as "last"`

方式二：`select last_name last`

好处有：方便，或者连接的时候有重名的情况

去重：

`select distinct department_id from employees;`

`+`在SQL中只有一个作用，就是作为运算符
如果两个运算数中有字符型，则首先尝试把字符型转换为数值型再进行加法，如果转换不成功则当作0处理。只要其中一方为`NULL`结果一定为`NULL`。

`IFNULL(expr1,expr2)`

如果值不是`NULL`就换成`expr1`，如果是`NULL`就换成`expr2`

```sql
##进阶2: 条件查询
/*
select
		查询列表
from
		表名
where
		筛选条件;

第一步走from，先看看有没有这个表，第二步看条件，最后才查询
*/
```

一：条件运算符：`> < = != <> >= <=`
二：按逻辑表达式筛选：`&& || !` 还有`and or not`

三：模糊查询：`like`，`between and`，`in`，`is null`

`MySQL`通配符：`%`任意多个字符，包含空字符
`_`任意一个字符
转义字符，`LIKE '_$_%' ESCAPE '$'`

判断`NULL`不能用`=`，只能用关键词`IS NULL`

安全等于 `<=>`可以判断`NULL`值也可以判断普通类型的值，用的少的原因是可读性比较差

```sql
##进阶3: 排序查询
/*
select
		查询列表
from
		表名
[where
		筛选条件]
[order by 排序列表 [asc|desc]];

1. asc是升序，desc是降序，默认是asc
2. order by 子句可以支持单个字段、多个字段、表达式、函数、别名
3. 一般放在查询语句的最后面，limit 子句除外
*/
```

ORDER BY 支持别名

```sql
##进阶4: 常见函数
/*
概念：类似于java的方法，将一组逻辑语句封装在方法体中，对外暴露方法名
好处：1. 隐藏了实现细节 2. 提高代码的重用性
调用：select 函数名（实参列表） [from 表]
特点：
	1. 叫什么（函数名）
	2. 干什么（函数功能）
	
分类：
	1. 单行函数
			concat、length、ifnull等
  2. 分组函数
			功能：做统计作用，又称为统计函数、聚合函数、组函数

1. asc是升序，desc是降序，默认是asc
2. order by 子句可以支持单个字段、多个字段、表达式、函数、别名
3. 一般放在查询语句的最后面，limit 子句除外
*/
```

#### 单行函数

##### 字符函数

1. `select length('john')`参数值的字节个数，注意，不是字符

2. `select concat(first_name, ' ', last_name) name from employees; `

3. `upper()`，`lower()`

4. `substr()`，`substring()`
   注意，SQL中索引是从1开始的，比如说陆展元：
   `select substr('李莫愁爱上了陆展元',7) out_put`

   比如说李莫愁
   `select substr('李莫愁爱上了陆展元',1,3) out_put`

5. `instr`返回子串第一次出现的索引，如果找不到返回0
   `select instr('','殷六侠')`

6. `trim()`跟java里面一样，去除 收尾的字符
   `select length(trim('   张翠山   ')) as out_put`

   `select trim('a', 'aaaaaaa张aa翠aaaaa山aaaaaaaa')`

7. `lpad`用指定的字符实现做填充指定长度
   `select lpad('殷素素',2,'*') as out_put`

8. `rpad`用指定的字符实现做填充指定长度
   `select rpad('殷素素',12,'*') as out_put`

9. `replace()`替换，都改
   `select replace('张无忌爱上了周芷若','周芷若','赵敏') as out_put`

##### 数学函数

1. `round()`四舍五入，负数的话先算绝对值四舍五入，然后再加山负号
   `select round(1,45);`
   `select round(-1.3);`
2. `ceil()`向上取整，返回>=该参数的最小整数
   `select ceil(-1.03);`
3. `floor()`向下取整，返回<=该参数的最大整数
   `select floor(-9.99);`
4. `truncate()`截断
   `select truncate(1,699,1);`
5. `mod()`取余运算
   `mod(a,b)`相当于`a - a/b*b`

##### 日期函数

1. `now()`返回当前系统日期+时间
   `select now();`
2. `curdate()`返回当前系统日期
   `select curdate();`
3. `curtime()`返回当前系统时间
   `select curtiem();`
4. 可以获取指定的部分，年、月、日、小时、分钟、秒
   `select YEAR(NOW()) as "year"`
   `select MONTH(NOW()) as month`
   `select MONTHNAME(NOW()) as month_name`
5. `str_to_data`将字符通过指定的格式转换成日期
   `select str_to_date('1998-2-3','%Y-%c-%d') as out_put;`
6. `date_format()`格式化输出日期

##### 流程控制函数

1. `if(expr1,expr2, expr3)`函数，相当于三元运算符，表达式1是判断条件，如果true则返回表达式2的值，如果false返回表达式3的值
   `if(10<5,'smaller','great')`
   `if(commission_pct is not null)`

2. `case()`函数的使用一：switch case的效果

   ```sql
   # 语法
   /*
   case 要判的字段或表达式
   when 常量1 then 要显示的值1或者语句1；
   when 常量2 then 要显示的值1或者语句2；
   else 要显示的值n或语句n
   end
   */
   select salary, department_id,
   case department_id
   when 30 then salary*1.1
   when 40 then salary*1.2
   else salary
   end as new_salary
   from employees;
   ```

   `case()`函数的使用二：类似于 多重if

   ```sql
   # 语法
   /*
   case 
   when 条件1 then 要显示的值1或语句1；
   when 条件1 then 要显示的值2或语句2；
   when 条件3 then 要显示的值1或语句3；
   else 要显示的值n或语句n
   end
   */
   select salary,
   case
   when salary>20000 then 'A'
   when salary>15000 then 'D'
   when salary>10000 then 'C'
   else 'D'
   end as salary_class
   from employees;
   ```

#### 分组函数

sum 求和、avg 平均值、 max 最大值、min 最小值、count计算个数

1. 简单实用

   ```sql
   select SUM(salary), round(avg(salary),2)
   ```

   

2. `sum`和`avg`只能用数值类型上
   `max`和`min`还能使用日期类型，实际上可以处理任何类型的
   `count`任何类型都支持，数的都是非空的

3. 以上分组函数都忽略`NULL`值

4. 和`distinct`搭配使用

   ```sql
   select sum(distinct salary), sum(salary) from employees;
   ```

5. `count()`函数的详细介绍

   ```sql
   # 统计行数，只要有一个字段非空就统计上了
   select count(*) from employees;
   # 统计行数
   select count(1) from employees;
   ```

   效率问题：

   `MYISAM`引擎下，`count(*)`效率最高
   `INNODB`引擎下，`count(*)`和`count(1)`效率差不多，比`count(字段)`高一些

6. 和分组函数一同查询的字段要求是`group by `后的字段，等下讲

#### 分组查询

```sql
##进阶5: 分组查询
/*
引入：查询每个部门的平均工资

语法：
		select 分组函数，列（要求出现在group by的后面）
		from 表
		[where conditions]
		group by 分组的列表
		[order by 子句]
注意：
		查询列表比较特殊，要求是分组函数和group by 后出现的字段
*/
```

分组查询中的筛选条件可以分成两类：

分组前筛选：数据源是原始表，group by 子句的前面，关键字where

分组后筛选：分组后的结果集，group by 子句的后面，关键字having

大招：

1. 分组函数做条件肯定刚在having子句后面
2. 能用分组前筛选，优先考虑分组前筛选，考虑性能问题

按表达式分组，group by 和 having 后面支持别名，但是泛用性不好只有MySQL支持，MySQL 的 where 后面不支持别名

group by 支持耽搁字段分组，多个字段分组（多个字段之间用逗号隔开，没有顺序），也支持表达式，但是不常用
分组之后还可以跟上排序，order by跟之前一样

#### 连接查询

```sql
##进阶6: 连接查询
/*
含义：又称多表查询，当长须你的字段来自于多个表时，就会用到连接查询

笛卡尔乘积： 表1 有m行，表2 有n航，结果有m*n行
发生原因：没有有效的连接条件
如何避免：添加有效的连接条件

分类：
		按年代分类：
		sql92标准：仅仅支持内连接
		sql99标准：支持内连接+外连接（左外和右外）+交叉连接
		按功能分类：
			内连接：
				等值连接
				非等值连接
				自连接
			外连接：
				左外连接
				右外连接
				全外连接(MySQL不支持)
			交叉连接
注意：
		查询列表比较特殊，要求是分组函数和group by 后出现的字段
*/
```

1. 多表的等值连接结果为多表的交集部分
2. n表连接，至少需要n-1个连接条件
3. 多表连接的顺序没有要求
4. 一般需要为表起别名，起了就必须用别名
5. 可以搭配前面的讲的分组，排序等

```sql
#等值连接语法：
select 查询列表
from table1 alias1, table2 alias2
where alias1.key = alias2.key
[and conditions]
[group by fields]
[having 分组后筛选]
[order by 排序字段]
```

一般为表起别名：为了缩短表名的长度和避免同名字段

```sql
/*
sql99标准
语法：
		select 查询列表 
		from 表1 别名 [join_type]
		join 表2 别名
		on 连接条件
		[where conditions]
		[group by field_list]
		[having conditions]
		[order by field_list]

join_type
		内连接：inner
		左外： left [outer]
		右外：	right [outer]
		全外：	full[outer]
		交叉：	cross
*/

```

##### 内连接

```sql
select 查询列表
from 表1 别名
[inner] join 表2 别名
on 连接条件
[
  where 筛选条件
  group by 分组列表
  having 分组后的筛选
  order by 排序列表
  limit 子句
]
```

等值连接特点：

1. 添加排序、分许、筛选
2. inner可以省略
3. 筛选条件放在where后面，链接条件放在on后面，提高分离性，便于阅读
4. inner join 连接和sql92语法中的等值连接效果是一样的，都是查询多表的交集
5. 内连接的结果=多表的交集
6. n表连接至少需要n-1个连接条件

##### 外连接

应用场景：一个表里面有，另一个表里面没有的

```sql
select 查询列表
from 表1 别名
left | right | full [outer] join 表2 别名
on 连接条件
[
  where 筛选条件
  group by 分组列表
  having 分组后的筛选
  order by 排序列表
  limit 子句
]
```

特点：

1. 外连接的查询结果为，主表中的所有记录
   如果从表中有和它匹配的，则显示匹配的值
   如果从表中没有和它匹配的，则显示null
   外连接查询结果=内连接结果+主表中有而从表中没有的记录
2. 左外连接，left join左边的是主表
   右外连接，right join右边的是主表
3. 左外和右外交换两个表的顺序可以实现同样的效果
4. 全外连接 = 内连接的结果+表1中有但表2中没有的+表2中有单表1中没有的

##### 交叉连接：笛卡尔乘积

sql92 vs sql99

功能：99功能多，而且有连接分离，提高可读性

#### 子查询/内查询

外面的语句可以是`insert`、`update`、`delete`、`select`等等，一般`select`比较多，外面的语句如果是select 语句，则此语句称为外查询或主查询

```sql
/*
##进阶7: 子查询
出现在其他语句中的select语句，称为子查询或内查询
外部的查询语句，称为主查询或外查询

分类：
按子查询出现的位置：
		select 后面：仅仅支持标量子查询
		from 后面：支持表子查询
		where或者having 后面：标量子查询 列子查询 行子查询（用的少）
		exists后面 （相关子查询）
按结果集的行列数不同：
		标量子查询（结果集只有一行一列）
		列子查询（结果集只有一列多行）/多行子查询
		行子查询（结果集有一行多列）
		表子查询（结果集一般为多行多列）
*/
```

##### 一、where或者having后面

1. 标量子查询（单行子查询）

   ```sql
   # 查询谁的工资比Abel高
   select *
   from employees
   where salary > (
   	select salary
       from employees
       where last_name = 'Abel'
   
   );
   
   # 返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id和工资
   select job_id
   from employees
   where employee_id = 141;
   
   select salary
   from employees
   where employee_id = 143;
   
   select last_name, job_id, salary
   from employees
   where job_id = (
   	select job_id
   	from employees
   	where employee_id = 141
   ) and salary > (
   	select salary
   	from employees
   	where employee_id = 143
   );
   
   # 案例3 返回公司工资最少的员工的last_name, job_id和salary
   select last_name,job_id, salary
   from employees
   where salary = (
   	select min(salary)
       from employees
   );
   
   # 案例4 查询最低工资大于50号部门最低工资的部门id和其最低工资
   select department_id, min(salary)
   from employees
   group by department_id
   having min(salary) > (
   	select min(salary)
       from employees
       where department_id = 50
   );

2. 列子查询（多子查询）
   要搭配`IN / NOT IN`等于列表中的任意一个、`ANY | SOME`、和子查询返回的某一个值比较`ALL`
   `ANY` 和`ALL`都可以被其它可读性更强的语句来替换 

   ```sql
   # 案例1: 返回location_id是1400或1700的部门中的所有员工姓名
   select distinct department_id
   from departments
   where location_id in (1400,1700);
   
   select last_name
   from employees
   where department_id in (
   	select distinct department_id
   	from departments
   	where location_id in (1400,1700)
   );
   
   # 案例2: 返回其它工种中比job_id为`IT_PROG`部门人意工资低的员工的：工号、姓名、job_id 以及salary
   select employee_id, last_name, job_id, salary
   from employees
   where salary <  (
   	select max(salary)
   	from employees
   	where job_id = 'IT_PROG'
   ) and job_id <> 'IT_PROG';
   
   # 案例2: 返回其它工种中比job_id为`IT_PROG`部门所有工资低的员工的：工号、姓名、job_id 以及salary
   select employee_id, last_name, job_id, salary
   from employees
   where salary <  (
   	select min(salary)
   	from employees
   	where job_id = 'IT_PROG'
   ) and job_id <> 'IT_PROG';
   ```

   

3. 行子查询（多行多列）
    一行多列或者多行多列，用的少，并且语法比较啰嗦

   ```sql
   # 案例1: 查询员工编号最小并且工资最高的员工信息
   select MIN(employee_id)
   from employees;
   
   select MAX(salary)
   from employees;
   
   select *
   from employees
   where employee_id = (
   	select MIN(employee_id)
   	from employees
   )and salary =(
   	select MAX(salary)
   	from employees
   );
   
   select *
   from employees
   where (employee_id,salary) = (
   	select MIN(employee_id),MAX(salary)
       from employees
   );
   ```

   

特点：

1. 子查询放在小括号内
2. 子查询一般放在条件的右侧
3. 标量子查询，一般搭配着单行操作符使用
   `> < >= <= = <>`
   列子查询一般搭配着多行操作符使用
   `IN ANY/SOME ALL`
4. 子查询的执行优先于主查询的进行

非法使用标量子查询：得到的结果不是一行一列：可能是空或者是有多行

##### 二、放在select后面的子查询

仅仅支持标量子查询

```sql
# 案例1： 查询每个部门的员工个数
select d.*, (
select count(*)
from employees e
where e.department_id = d.department_id
)
from departments d;

# 案例2： 查询员工号=102的部门名
select (
	select department_name
    from departments d
    where d.department_id = e.department_id
)
from employees e
where e.employee_id = 102;

select department_name "部门"
from departments d
inner join employees e
on d.department_id = e.department_id
where e.employee_id = 102;
```

##### 三、from 后面（跟表就完事儿了）

就是把我们查询的结果充当一张表，要求这张表必须起别名就完事儿。

```sql
# 案例： 查询每个部门的平均工资等级
select avg(salary), department_id
from employees
group by department_id;

select ag_dep.*, g.grade
from(
	select avg(salary) ag, department_id
    from employees
    group by department_id
) ag_dep
inner join job_grades g
on ag_dep.ag between g.lowest_sal and highest_sal

```

##### exists后面（相关子查询）

```sql
/*
语法
exists （完整的查询语句）
结果：1或者0
*/
# 案例1:查询有员工的部门名
select department_name
from departments d
where exists (
	select *
    from employees e
    where d.department_id = e.department_id
);

select department_name
from departments d
where d.department_id in (
	select department_id
    from employees
    
)
```

#### 分页查询

```sql
/*
##进阶8: 分页查询
应用场景：当我们要显示的数据一页显示不全，需要分页提交sql请求
语法：
		select 查询列表
		from 表
		[join type join 表2
		on 连接条件
		where 筛选条件
		group by 分组字段
		having 分组后的筛选
		order by 排序的字段]
		limit [offset,] size;

offset 要是条目的起始索引（起始索引从0开始）
size 要显示的条目个数
*/
# 案例1:查询前五条员工信息
select * from employees LIMIT 0,5;
select * from employees limit 5;

# 案例2:查询第11条--第25条
select * from employees LIMIT 10,15;

# 案例3:有奖金的员工信息，并且工资较高的前十名显示出来
select salary
from employees
where commission_pct is not null
order by salary desc
LIMIT 10;
```

特点：

1. limit语句放在查询语句的最后，无论是执行上还是语法上
2. 公式
   要显示的页数 page，每页的条目数size
   select 查询列表
   from 表
   limit (page - 1) * size

#### 联合查询(UNION)

```sql
/*
##进阶9: 联合查询
union 联合 合并：将多条查询语句的结果合并成一个结果
引入的案例：查询部门编号>90或者邮箱包含a的员工信息
select * from employees where email like '%a%' or department_id > 90;

select * from employees where email like;
union
select * from employees where department_id > 90;

语法：
	查询语句1
	UNION [ALL]
	查询语句2
	...

应用场景：
要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致
*/
```

特点：

1. 多条查询语句的查询列数是一致的
2. 字段名默认是第一条语句的字段名，要求每一列的类型和顺序最好一致
3. UNION关键字默认去重，如果不想要去重可以追加关键字ALL



##### 查询小结：

```sql
select 查询列表			 7
from 表1 别名				1
连接类型 join 表2		 2
on 连接条件					  3
where 筛选条件 				4
group by 分组列表			5
having 筛选						6
order by 排序列表			8
limit 其实条目索引，条目数； 	9
```



### DML (Data Manipulation Language)

#### 插入：insert

**方式一：**

```sql
/* 表已经存在了，往里加数据
语法：
表名
列名
新值

insert into 表名 (列名,...) values(值1,...);
*/
```

要求：

1. 插入的值的类型要于列的类型一致或兼容
2. 不可以为NULL的列必须插入值，可以为NULL的列如何插入值？
   方式一：列名有，值写NULL
   方式二：列名里就不写
3. 列的顺序是否可以调换？可以的没有问题
4. 列数和值的个数必须一致
5. 可以省略列名，默认是所有列，而且列的顺序和表的顺序一致

**方式二：**

`insert into 表名 set 列名=值，列名=值,...`

两种方式PK

1. 方式一支持插入多行，方式二不支持

2. 方式一支持子查询，方式二支持

#### 修改：update

```sql
/*
1. 修改单表的记录
语法：
update 表名 set 列=新值,列=新值,...
where 筛选条件
2. 修改多表的记录，级联修改
语法：
sql92 语法
update 表1 别名，表2 别名
set 列=值,...
where 连接条件
and 筛选条件；
sql99语法
update 表1 别名
inner | left | right join 表2 别名
on 连接条件
set 列=值，...
where 筛选条件；
*/
```



#### 删除：delete

```sql
/*
方式一：delete
语法：
delete 表名 set 列=新值,列=新值,...
where 筛选条件
1. 单表的删除
delete from 表名 where 筛选条件
*/

delete from beauty where phone like '%9';

delete b
from beauty b
inner join boys bo on b.boyfriend_id = bo.id
where bo.boyName = '张无忌';
/* 2. 多表的删除
方式二：truncate
语法 truncate table 表名;

语法：
sql92 语法
delete 表1 别名，表2 别名
where 连接条件
and 筛选条件；
sql99语法
delete 表1的别名，表2的别名
from 表1 别名
inner | left | right join 表2 别名
on 连接条件
where 筛选条件；
*/

```

`delete` vs `truncate`

1. delete 可以加where 条件，truncate 不能
2. truncate 删除，效率高一丢丢
3. 假如要删除的表中有自增长列，如果用delete删除后，在插入数据，自增长的列从断电开始，二truncate删除后，再插入数据，自增长的列从1开始
4. truncate删除没有返回值，delete删除有返回值
5. truncate删除不能回滚，delete可以回滚

### DDL (Data Definition Language)：对库和表的管理

##### 一、库的管理

创建 、修改、删除

1. 库的创建
   语法：`create database DB_NAME;`
   `create database [if not exists] DB_NAME;`
2. 库的修改
   一般不修改，`RENAME DATABASE books to NEW_NAME`，以前能用，现在不行了
   可以更改库的字符集
3. 库的删除
   `drop database if exists books;`

##### 二、表的管理

1. 创建：`create `

   ```sql
   create table TABLE_NAME(
   		FIELD_NAME TYPE[(), contraints],
     	FIELD_NAME TYPE[(), contraints],
     	FIELD_NAME TYPE[(), contraints],
     	FIELD_NAME TYPE[(), contraints],
     	...
     	FIELD_NAME TYPE[(), contraints],
   );
   ```

   

2. 修改：`alter table+ 表名 ｜change | modify | add | drop`
   可以修改什么？alther table + 表名

   ```sql
   2.1 列名 alter table book change column publishdate pubDate DATETIME;
   2.2 列的类型或约束 alter table book modify column pubDate TIMESTAMP;
   2.3 添加新的列 alter table author ADD column annual double; 
   2.4 删除列 alter table author drop column annual;
   2.5 修改表名 alter table author rename to book_author;
   ```

   

3. 删除：`drop table [if exists] 表名`

通用的写法

```sql
DROP DATABASE IF EXISTS old_database;
CREATE DATABASE new_database;

DROP TABLE IF EXISTS old_table;
CREATE TABLE new_table();
```

4. 表的复制

   ```sql
   #1. 仅仅复制表的结构
   create table dst_table like src_table;
   #2. 复制表的结构+数据
   create table dst_table
   select * from src_table;
   #3. 只复制部分数据
   create table dst_table
   select column_names
   from src_table
   where conditions
   
   #4. 仅仅复制某些字段
   create table dst_table
   select column_names
   where 0;
   ```

##### 常见数据类型

1. 数值型：
   整型：`tinyint`、`smallint`、`mediumint`、`int`、`bigint`

   1.1 如果不设置无符号`unsigned`就是有符号，如果插入的数据超过范围会报out of range异常
   1.2 存临界值如果不设置长度，会有默认长度。
   1.3 长度代表结果显示长度。如果要显示加关键词`zerofill`，但是会默认无符号。
   小数：定点型`dec(m,d)`或者`decimal(m,d)`、浮点型`float(m,d)`或者`double(m,d)`
   1.4 M和D是啥？D是小数点后的位数，M是整数部位和小数部位的个数，超出范围添加临界值
   1.5 M和D都可以省略，如果是decimal，则M默认为10，D默认为0
   1.6 定点型的进度较高，如果要求插入数值的京都较高，如货币运算等则考虑使用

   原则：所选择的类型越简单越好，占用空间越小越好

2. 字符型：
   较短的文本：`char`、`varchar`

   2.1`char(M)`和`varchar(M)`中M都是最大的字符数，`char`固定长度，比较耗费空间但是效率高，`varchar`比较节省空间但是效率较低。
   2.2 `char(M)`中的M可以省略，默认为1，但是`varchar()`不能省略。
   较长的文本：`text`、`bloc(较长的二进制数据)`
   2.3 其它的还有`binary`

3. 日期型：必须用单引号引起来
   date只保存日期
   time 只保存时间
   year 只保存年
   datetime 8字节，范围1000-9999，不受时区影响
   timestamp 4字节，范围1970-2038，受时区影响

##### 常见约束

含义：一种限制，用于限制表中的数据，为了保证表中数据的准确和可靠性

分类：六大约束

1. `NOT NULL`：非空，用于保证该字段的值不能为空，比如姓名、学号等
2. `DEFAULT`：默认，用于保证该字段有默认值，比如性别
3. `PRIMARY KEY`：主键，用于保证该字段的值具有唯一性，并且非空，比如学号，员工编号
4. `UNIQUE`：唯一，用于保证该字段的值具有唯一性，可以为空。比如座位号
5. `CHECK`：检查约束，`MySQL`不支持，语法不报错，但是没作用。
6. `FOREIGN KEY`：外键，用于简直两个表的关系的，用于保证该字段的值必须来自于主表的关联列的值。
   在从表添加外键约束，用于引用主表中某列的值，比如学生表的专业编号，员工表的部门编号，员工表的工种编号。

添加约束的时机：创建表时，修改表时，总之都在添加数据之前

约束的添加分类：
列级约束：六大约束语法上都支持，但是外键约束没有效果

语法：直接在字段名和类型后面追加 约束类型即可。

只支持：默认、非空、主键、唯一

表级约束：除了非空、默认，其它都支持

```sql
CREATE TABLE stuinfo(
	id INT,
  stuname VARCHAR(20),
  geneder CHAR(1),
  seat INT,
  age INT,
  majorid INT,
  
  CONSTRAINT pd PRIMARY KEY(id), #主键
  CONSTRAINT uq UNIQUE(seat), #唯一键
  CONSTRAINT ck CHECK(gender in ('M','F')), #主键
  CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id) #外键
)
```

通用写法：

```sql
CREATE TABLE IF NOT EXISTS stuinfo(
	id INT PRIMARY KEY,
  stuname VARCHAR(20) NOT NULL,
  gender CHAR(1),
  age INT DEFAULT 18,
  seat INT UNIQUE,
  majorid INT,
  CONSTRAINT fk_stuinfo_major FOREIGN KEY(major) REFERENCES major(id)
);
```

主键和唯一的对比

​				保证唯一性		是否允许非空		一个表中可以有多少个		知否允许组合

主键		yes						no						at most one						yes, not recommended

唯一		yes						yes						muliple								yes, not recommended

外键：

1. 要求在从表设置外键关系
2. 从表的外键列的类型和主表的关联列的类型要求一致或者兼容，名称无要求
3. 住表的关联列必须是一个key（一般是主键或者唯一键）
4. 插入数据时，应该先插入主表的数据，删除数据时，应该先删除从表，再删除主表



标识列：又称为自增长列

含义：可以不用手动插入值，系统提供默认的序列值

一、创建表时设置标识列

```sql
CREATE TABLE tab_identity(
	id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20)
);
```

二、特点

1. 标识列必须和主键搭配吗？不一定，但要求是一个key
2. 一个表可以有几个标识列？至多一个
3. 表示列的类型，只能是数值型，一般是int
4. 表示列可以通过`SET auto_increment_increment=3`设置步长
   可以通过手动插入值，设置起始值

### TCL (Transaction Control Language)

事务控制语言

#### 事务

一个或一组sql语句组成一个执行单元，这个执行单元要么全部执行，要么全部不执行。

由单独单元的一个或多个SQL语句组成，在咋饿个单元中，每个MYSQL语句是相互依赖的。而整个单独单元作为一个不可分割的整体，如果单元中某条SLQ语句一旦执行失败或产生错误，整个单元将会回滚。所有收到影戏那个的数据将返回到事务开始前的装袋；如果单元中的所有个SQL语句均执行成功，则食物被顺利执行。

事务的ACID性质：

* 原子性(Atomicity)：原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生
* 一致性（Consistency）：事务必须是数据库从一个一执行状态变换到另一个一执行状态，例如：转账的前后两个账户的总金额要一样
* 隔离性（Isolation）：事务的隔离性是指一个事务的执行不能被其它事务干扰， 即一个事物内部的操作及使用的数据对兵法的事务是隔离的，并发执行的哥哥事务之间不能互相干扰。很多时候达不到，通过隔离级别来控制
* 持久性（Durability）：持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来的其它操作和数据库故障不应该对其有任何影响。

事务的创建：

隐式事务：事务没有明显的开启和结束标记

比如insert、update、delete语句

显式事务：事务具有明显的开启和结束标记

前提：必须先设置自动提交功能为禁用
`set autocommit=0;`

步骤一：开启事务

`set autocommit=0;`

`start transaction;`可选的

步骤二：便携事务中的sql语句（`select`、`insert`、`update`、`delete`）

语句1；

语句2；

...

步骤三：结束事务

`commit;`提交事务

`rollback;`回滚事务

#### 视图

行和列的数据都来自自定义视图的查询中使用的表， 并且是在使用视图的时候动态生成的，只保存了sql逻辑， 不保存查询结果

含义：虚拟表，和普通表一样使用，mysql15.1版本出现的新特性，是通过表动态生成的数据

比如：舞蹈版和普通班级的对比

##### 创建视图

语法：

```sql
CREATE VIEW 视图名
AS
查询语句；
```

#### 视图的好处

* 重用sql语句
* 简化复杂的sql操作
* 保护数据，提高安全性

##### 修改视图

```sql
# 方式一
create or replace view 视图名
as
查询语句；
# 方式二
alter view 视图名
as
查询语句；
```

##### 删除视图

```sql
drop view 视图名,视图名,...;
```

##### 查看视图

```sql
desc myv3;
show create view myv3;
```

##### 视图的更新

可以往视图里面insert数据，原始表也会受到影响。一般来说给视图增加只读权限

具备一下特点的视图不允许更新：

1. 包含一下关键字的sql语句：分组函数、`distinct`、`group by`、`having`、`union`或者`union all`
2. 常量视图：`create or replace view myv2 as select 'john';`
3. `select`中包含子查询
4. `join`
5. `from`一个不能更新的表
6. where中的查询使用了from中子查询



### 变量、存储过程、函数

#### 变量

* 系统变量：变量有系统提供，不是用户定义，属于服务器层面
  查看所有的系统变量
  * 全局变量 `SHOW GLOBAL VARIABLES;`
    作用域：服务器每次启动将为所有的全局变量赋初始值，针对于所有的会话（连接）有效，但不能跨重启。
  * 会话变量`SHOW SESSION VARIABLES;`
    作用域：仅仅针对于当前会话（连接）有效
  * 查看指定的某个系统变量的值`select @@global.VARIABLE_NAME;`
  * 为某个系统变量赋值`set global [session] VARIABLE = VALUE`或者`set @@global [session].VARIABLE_NAME=VALUE;`
* 自定义变量：
  * 用户变量
    作用域：针对于当前会话（连接）有效，同于会话变量的作用域
    声明并初始化 `SET @VARIABLE_NAME=Value;`或`SET @VARIABLE_NAME:=Value;`或`SELECT @VARIABLE_NAME:=Value;`
  * 局部变量
    作用域：仅仅在定义他的`begin`、`end`中有效，必须是`begin`、`end`中的第一句话
    `declare variable_name type;`或者`declare variable_name type default value;`

#### 存储过程和函数

类似于Java中的方法。

好处：

1. 提高代码的重用性
2. 简化操作

##### 存储过程

含义：一组预先编译好的SQL语句的集合，理解成批处理语句

1. 提高代码的重用性
2. 简化操作
3. 减少了贬义词书并且减少了和数据库服务器的连接次数，提高了效率

一、创建语法

```sql
CREATE PROCEDURE 存储过程名(参数列表)
BEGIN
		存储过程体（一组合法的的SQL语句）
END
```

注意

1. 参数列表包含三部分
   参数模式 参数名 参数类型，例如`IN stuname VARCHAR(20)`

   参数模式：

   `IN`：该参数可以作为输入，也就是该参数需要调用方传入值

   `OUT`：该参数可以作为输出，也就是该参数可以作为返回值

   `INOUT`：该参数及可以作为输入又可以作为输出，也就是该参数既需要传入值，和可以返回值

2. 如果存储过程体仅仅只有一句话，`BEGIN END`可以省略
   存储过程体中的每条SQL语句的结尾要求必须加分号。
   存储过程的结尾可以用`DELIMITER`重新设置
   语法：`DELIMITER $`

二、调用语法
`CALL 存储过程名(实参列表);`

##### 函数

含义：一组预先编译好的SQL语句的集合，理解成批处理语句

1. 提高代码的重用性
2. 简化操作
3. 减少了贬义词书并且减少了和数据库服务器的连接次数，提高了效率

区别：

存储过程：可以有0个返回值，也可以有多个返回，适合做批量插入、批量更新

函数：有且仅有1个返回，适合做处理数据后返回一个结果

一、创建语法

```sql
CREATE FUNCTION 函数名(参数列表) RETURNS 返回类型
BEGIN
		函数体
END
```

注意：

1. 参数列表包含两个部分，参数名，参数类型
2. 函数体：肯定会有return语句，如果没有也不会报错
   如果return语句没有放在函数体的最后也不会报错，但不建议
3. 函数体中仅有一句话，则可以省略`BEGIN`、`END`
4. 使用`DELIMITER`语句设置结束标记



二、调用语法

`select 函数名(参数列表)`



### 流程控制结构

#### 顺序结构

程序从上往下依次进行

#### 分支结构

程序从两条或多条路径中选择一条去执行

1. `if`函数，能够实现简单的双分支
   `select if(expr1, expr2, expr3)`
   如果表达式1成立，则返回表达式2的值，否则返回表达式3的值

2. `case`结构
   情况一：类似于Java中的swith语句，一般用于实现等值判断

   ```sql
   CASE 变量｜表达式｜字段
   WHEN 要判断的值 THEN 返回的值1或语句1;
   WHEN 要判断的值 THEN 返回的值2或语句2;
   ...
   ELSE 要返回的值n或语句n;
   END CASE;
   ```

   情况二：类似于Java中的多重if语句，一般用于实现区间判断

   ```sql
   CASE 变量｜表达式｜字段
   WHEN 要判断的条件1 THEN 返回的值1或语句1;
   WHEN 要判断的条件2 THEN 返回的值2或语句2;
   ...
   ELSE 要返回的值n或语句n;
   END CASE;
   ```

   特点：

   1. 可以作为表达式，嵌套在其它语句中使用，可以放在任何地方，`BEGIN END`中或者外面
      也可以作为独立的语句去使用，只能放在`BEGIN END`中
   2. 如果WHEN中的值满足货条件成立，则执行对应的THEN后面的语句，并且结束CASE，如果都不满足，则执行ELSE中的语句或值
   3. ELSE可以省略，如果省略了，并且所有WHEN条件都不满足，则返回NULL

3. `if`结构：实现多重分支

   ```sql
   IF 条件1 then 语句1；
   elseif 条件2 then 语句2；
   ...
   [else 语句n;]
   end if;
   ```

   应用在`BEGIN END`中

#### 循环结构

程序在满足一定条件的基础上，重复执行一段代码

1. while

   ```sql
   [label:] while 循环条件 do
   		循环体;
   end while [label];
   ```

2. loop

   ```sql
   [label:] loop
   		循环体;
   end loop [label];
   ```

   可以用来模拟简单的死循环

3. repeat

   ```sql
   [label:] repeat
   		循环体；
   until 结束循环的条件
   end repeat [label];
   ```

   

循环控制：

1. `iterate`类似于`countinue`，继续，结束本次循环，继续下一次
2. `leave`类似于`break`，跳出，结束当前所在的循环
