### 第1章JDBC概述

#### 数据的持久化

* 绝大多数情况表示，把内存中的数据保存到硬盘中
* 数据量大、多表查询

#### Java中的数据存储技术

* Java中，数据库存储可以分为如下几类
  * **JDBC**直接访问数据库
  * JDO（）技术
  * 第三方 **O/R工具**，如Hibernate、MyBatis等
* JDBC是java访问数据库的基石，Hibernate、MyBatis等只是对JDBC做了更好的封装

#### JDBC介绍

* JDBC（Java Database Connectivity）**独立与特定DBMS、通用的SQL数据库村去和操作的接口**。本质上一组API(`java.sql`、`javax.sql`)，最终使用的还是SQL。
* 一组规范，是使用Java程序操作数据库规范。使用具体的数据库时，需要安装具体的JDBC驱动。

#### JDBC体系结构

* JDBC接口（API）包括两个层次：
  * 面向应用的API：Java API，抽象接口，提供应用程序开发人员使用（连接数据库，执行SQL语句，获得结果）
  * 面向数据库的API：Java Driver API，供开发商开发数据库驱动应用程序

#### JDBC程序编写步骤

导入`java.sql`包 ==> 纯Java驱动 ==> 创建Connection对象 ==> 创建Statement对象 ==> 执行SQL语句

根据是否有结果集分支，之后关闭Statement对象，关闭Connection对象，结束

### 第2章获取JDBC连接
