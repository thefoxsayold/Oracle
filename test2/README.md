# 实验2：用户管理 - 掌握管理角色、权根、用户的能力，并在用户之间共享对象。

## 实验内容：

### - 1.在pdborcl插接式数据中创建一个新的本地角色con_res_view，该角色包含connect和resource角色，同时也包含CREATE VIEW权限，这样任何拥有con_res_view的用户就同时拥有这三种权限。

- 第1步：以system登录到pdborcl，创建角色con_res_zb和用户new_zb，并授权和分配空间：
#### sqlplus system/123@pdborcl;   以system角色登录到数据库。
> SQL> show user;<br>
USER 为 "SYSTEM"<br>
>>SQL*Plus: Release 12.1.0.2.0 Production on 星期三 10月 24 14:21:41 2018<br>
>>Copyright (c) 1982, 2014, Oracle.  All rights reserved.<br>
>>上次成功登录时间: 星期三 10月 24 2018 14:15:19 +08:00<br>
>>连接到:<br>
>>Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production<br>
>>With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options<br>



#### CREATE ROLE con_res_zb;   创建角色。
>角色已创建。

#### GRANT connect,resource,CREATE VIEW TO con_res_zb;  角色包含connect和resource角色，同授予CREATE VIEW权限。
>授权成功。

### - 2.创建角色之后，再创建用户new_user，给用户分配表空间，设置限额为50M，授予con_res_view角色。

####  CREATE USER new_zb IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;  创建新的用户
>用户已创建。

#### ALTER USER new_zb QUOTA 50M ON users;  给用户分配空间
>用户已更改。

#### GRANT con_res_zb TO new_zb;    让 new_zb包含con_res_zb角色。
>授权成功。

### -3.最后测试：用新用户new_zb连接数据库、创建表，插入数据，创建视图，查询表和视图的数据。

#### sqlplus new_zb/123@pdborcl;
>show user;<br>
>USER 为 "NEW_ZB"
>>SQL*Plus: Release 12.1.0.2.0 Production on 星期三 10月 24 14:44:56 2018<br>
Copyright (c) 1982, 2014, Oracle.  All rights reserved.<br>
上次成功登录时间: 星期三 10月 24 2018 14:40:51 +08:00<br>
连接到:<br>
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production<br>
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options<br>

