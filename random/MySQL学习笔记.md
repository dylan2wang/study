## MySQL学习笔记(Win&Linux)

不同平台的mysql的启动方式有所不同，但是对于数据库操作的命令大致一样；

#### Mac 下终端启动mysql的流程

```
$ mysql.server start	// 启动mysql服务
$ mysql -uroot -p	// 回车后输入密码，本机是我名字的拼音小写
操作数据库
mysql> exit	// 退出数据库
$ mysql.server stop	// 关闭mysql服务
```

#### 常用终端命令

net 命令貌似是windows上有效的

1. $ net start mysql	// 启动mysql服务
2.	$ net stop mysql 	// 关闭mysql服务 
3. $ mysql -hlocalhost -uroot -p	// 连接mysql，-h 表示服务器，localhost指明是本地服务器，-u表示用户，赋予的参数是root，-p表示密码，上例表示没有密码， 如果密码是12345则改为-p12345即可。

Linux 启动顺序

```
$ sudo service mysql start
$ mysql

```

Linux 关闭顺序

```
$ mysql exit
$ sudo service mysql stop
```


#### Linux 下MySQL操作命令

- Windows和它操作几乎一样，只有个别地方不同；
- 执行mysql命令，必需以 ';'  结尾

```
1.终端启动 MySQL：/etc/init.d/mysql start // 不同平台启动方式不一样，Mac的启动方式，见上面

2.登录 MySQL：mysql -uroot -p (用 root 账户登录),然后输入密码 

3.查看所有的数据库名字：show databases; 

4.选择一个数据库操作： use database_name; 

5.查看当前数据库下所有的表名：show tables; 

6.创建一个数据库：CREATE DATABASE database_name; 

7.删除一个数据库：DROP DATABASE database_name; 

8.创建一个表: CREATE TABLE mytest( uid bigint(20) NOT NULL PRIMARY KEY, uname varchar(20) NOT NULL); 

9.删除一个表: DROP TABLE mytest; 

10.SQL 插入语句：INSERT INTO table_name(col1,col2) VALUES(value1,value2); 

11.SQL 更新语句：UPDATE table_name SET col1='value1',col2='value2' WHERE <condition>; 

12.SQL 查询语句：SELECT * FROM table_name WHERE.......

13.SQL 删除语句：DELETE FROM table_name WHERE... 

14.增加表结构的字段：ALTER TABLE table_name ADD COLUMN field1 date ,ADD COLUMN field2 time... 

15.删除表结构的字段：ALTER TABLE table_name DROP field1; 

16.查看表的结构：SHOW COLUMNS FROM table_name; 

17.limit 的使用：SELECT * FROM table_name LIMIT 3;   //每页只显示 3 行 

SELECT * FROM table_name LIMIT 3,4;   //从查询结果的第三个开始，显示四项结果。 此处可很好的用来作分页处理。 

18.对查询结果进行排序: SELECT * FROM table_name ORDER BY field1,orderby field2;多重排序 

19.退出 MySQL:exit; 

20.删除表中所有数据： truncate table 数据表名称 （不可恢复）
```

#### 常用数据类型

**字符串类型**

```
- char
- varchar(int)
- TEXT 或 BLOB	// 最大64K，BLOB大小写敏感，而TEXT不是大小写敏感的
- MEDIUMTEXT 或 MEDIUMBLOB	// 最大16M，MEDIUMBLOB大小写敏感，而MEDIUMTEXT不是大小写敏感的；
```


#### 常用sql语句

**表处理语句，创建，修改属性，重命名，创建主键，删除字段**

```
- CREATE TABLE table_name(col1 type1 [NOT NULL] [PRIMARY KEY],col2 type2 [NOT NULL],..)

========== 例子 ==========
普通
CREATE TABLE test(name varchar(25), password varchar(25));

主键自增长
create table test(id int primary key auto_increment, password varchar(25));

增加表结构的字段：
- ALTER TABLE table_name ADD COLUMN field1 date ,ADD COLUMN field2 time... 

删除字段
- ALTER TABLE table_name DROP field1;

修改表属性-修改名称和数据类型
- ALTER TABLE table_name CHANGE oldname newname type;

```
**添加索引，删除索引（主键索引，unique索引，普通索引）**

索引部分的内容没有经过实验验证

```
添加unique索引
ALTER TABLE 'table_name' ADD UNIQUE('name')

删除unique索引
ALTER TABLE 'table_name' DROP INDEX name
```


**查询数据**

```
- SELECT * FROM table_name WHERE <conditions>
- SELECT COUNT(*) FROM table_name	// 输出数据条目个数
```

**limit 的用法**

```
SELECT * FROM table LIMIT m,n // m 表示从第几行开始输出，n表示要输出多少行

输入不同种类的参数，其用法略有不同。

用法1:
mysql> SELECT * FROM table LIMIT 5,10; // 表示从table的第5行开始输出，一共输出10行，即输出6-15行

用法2:
mysql> SELECT * FROM table LIMIT 95,-1; // 表示检索table的 96-last 行

用法3:
mysql> SELECT * FROM table LIMIT 5; // 表示检索前5行记录


```

#### 导出数据库中的数据

导出数据库中的数据，只需要启动mysql服务器就可以，无需进入数据库；

```
$ mysqldump -u <user_name> -p <password> <database_name> > database_name.sql
```
注：上面的密码，一般在敲这行命令的时候不需要输入，回车后，系统会提示输入；

例子：已知数据库名称：danmu, 用户名 root ，数据库密码：wangruidong

```
$ mysqldump -u root -p danmu > danmu.sql
```

参考：http://www.cnblogs.com/jiunadianshi/archive/2011/04/20/2022334.html

#### 导入数据到数据库

'''
1.选定一个数据库后；
source /tmp/path/data.sql
注意：sql文件是绝对路径
'''