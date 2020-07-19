---
layout: post_layout
title: 「数据库」MySQL基础语法
time: 2018年05月13日 星期一
location: 北京
pulished: true
excerpt_separator: "```"
---
1. 创建数据库
```sql
    CREATE DATABASE database_name;
```

<!--more-->

2. 查看创建好的数据库
```sql
    SHOW DATABASE database_name;
    SHOW DATABASES;
```
3.  删除数据库
```sql
     DROP DATABASE database_name;
```
4. 查看数据库引擎
```sql
     SHOW ENGINES;
```
5. 查看默认引擎
```sql
     SHOW VARIABLES LIKE '%storage_engine%';
```
6. 创建表
    ```sql
     CREATE TABLE table_name
     (
     字段1  数据类型(列级别约束条件) DEFAULT 默认值,
     字段2  数据类型(列级别约束条件) DEFAULT 默认值
     表级别的约束条件
     );
    ```
7. 查看表                              
     ```sql
     SHOW TABLES;
     ```
8. 主键约束
    > 字段 数据类型(列级别约束) PRIMARY KEY DEFAULT 默认值
     ```sql
     id int(8) PRIMARY KEY DEFAULT 0,
     ```
    > 或者表级别约束：
    ```sql
     CONSTRAINT PRIMARY KEY (id, name)
    ```
9. 外键约束
```sql
     CREATE TABLE mldn.tb_test(
      tb_id int(8) UNIQUE DEFAULT 0,
      tb_name varchar(32) NOT NULL,
      tb_c_id int(8),
      CONSTRAINT PRIMARY KEY (tb_id),
      CONSTRAINT fk_tb_id FOREIGN KEY(tb_c_id) REFERENCES mldn.tb_c(c_id)
      )ENGINE=innodb DEFAULT CHARSET=GB2312;

      create table mldn.tb_c(
      c_id int(8)  PRIMARY KEY AUTO_INCREMENT,
      c_name varchar(32)
      )ENGINE=InnoDB DEFAULT CHARSET=GB2312;

      -- 删除外键约束
      ALTER TABLE tb_c DROP FOREIGN KEY fk_tb_id;
```

10. 查看表基本结构
```sql
   DESC tb_c;
   DESCRIBE tb_test;
```

11. 查看表详细结构
```sql
   SHOW CREATE TABLE tb_test ;
```

12. 修改表名
```sql
    ALTER TABLE tb_test RENAME tb_a;
```

13. 修改字段的数据类型
```sql
    ALTER TABLE tb_a MODIFY tb_name varchar(64);
```

14. 修改字段名
```sql
   ALTER TABLE tb_a CHANGE tb_name tb_username varchar(32);
```

15. 添加一列字段
```sql
   ALTER TABLE tb_a ADD dept_id varchar(32) NOT NULL  FIRST;
   ALTER TABLE tb_a ADD dept_id varchar(32) NOT NULL AFTER tb_username;
```

16. 删除一列
```sql
   ALTER TABLE tb_a DROP tb_username;
```

17. 修改字段的排列位置
```sql
   ALTER TABLE tb_a MODIFY tb_username varchar(32) FIRST;
   ALTER TABLE tb_a MODIFY tb_username varchar(32) AFTER tb_id;
```

18. 更改存储引擎(无外键约束可改)
```sql
    ALTER TABLE tb_a ENGINE=MyISAM;
```

19. 删除表
```sql
   DROP TABLE IF EXIST tb_c;
```

20. 运算符     
```sql
   -- 算术运算符 加减乘除取余数
   SELECT tb_id+1,tb_id+3-5,tb_id-4+9 FROM tb_a;
```
比较运算符
![](/images/20180513/2.jpg)

21. group by + having + limit
```sq
   SELECT id,SUM(price),GROUP_CONCAT(name) FROM tb_fruit WHERE id>0 GROUP BY id HAVING COUNT(name)>1 ORDER BY name DESC LIMIT 4,3;
   SELECT * FROM tb_fruit WHERE id>0 GROUP BY id,name;
```
