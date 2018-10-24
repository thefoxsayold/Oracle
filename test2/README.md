# 实验2：用户管理 - 掌握管理角色、权根、用户的能力，并在用户之间共享对象。

## 实验内容：

### - 1.在pdborcl插接式数据中创建一个新的本地角色con_res_view，该角色包含connect和resource角色，同时也包含CREATE VIEW权限，这样任何拥有con_res_view的用户就同时拥有这三种权限。

- 第1步：以system登录到pdborcl，创建角色con_res_zb和用户new_zb，并授权和分配空间：
#### 以system角色登录到数据库。
>sqlplus system/123@pdborcl;   
>> SQL> show user;<br>
>>USER 为 "SYSTEM"<br>
>>>SQL*Plus: Release 12.1.0.2.0 Production on 星期三 10月 24 14:21:41 2018<br>
>>>Copyright (c) 1982, 2014, Oracle.  All rights reserved.<br>
>>>上次成功登录时间: 星期三 10月 24 2018 14:15:19 +08:00<br>
>>>连接到:<br>
>>>Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production<br>
>>>With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options<br>



#### 创建角色。
>CREATE ROLE con_res_zb;
>>角色已创建。

#### GRANT connect,resource,CREATE VIEW TO con_res_zb;  角色包含connect和resource角色，同授予CREATE VIEW权限。
>授权成功。

### - 2.创建角色之后，再创建用户new_user，给用户分配表空间，设置限额为50M，授予con_res_view角色。

####    创建新的用户new_zb
>CREATE USER new_zb IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
>>用户已创建。

####   给用户分配空间
>ALTER USER new_zb QUOTA 50M ON users;
>>用户已更改。

#### 让 new_zb包含con_res_zb角色。
>GRANT con_res_zb TO new_zb;    
>>授权成功。

### -3.最后测试：用新用户new_zb连接数据库、创建表，插入数据，创建视图，查询表和视图的数据。

#### 以用户new_zb连入数据库
>sqlplus new_zb/123@pdborcl;
>>show user;<br>
>>USER 为 "NEW_ZB"
>>>SQL*Plus: Release 12.1.0.2.0 Production on 星期三 10月 24 14:44:56 2018<br>
>>>Copyright (c) 1982, 2014, Oracle.  All rights reserved.<br>
>>>上次成功登录时间: 星期三 10月 24 2018 14:40:51 +08:00<br>
>>>连接到:<br>
>>>Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production<br>
>>>With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options<br>

#### 创建表并插入数据
>创建表student<br>
>>CREATE TABLE student (id number,name varchar(50),sex varchar(50));<br>
>>>表已创建。<br>
>插入数据 “1 zhang male”、 “2 liu female”、“3 wang male”<br>
>>SQL> INSERT INTO student(id,name,sex)VALUES(1,'zhang','male');<br>
>>>已创建 1 行。<br>
>>SQL> INSERT INTO student(id,name,sex)VALUES(2,'liu','female');<br>
>>>已创建 1 行。<br>
>>SQL> INSERT INTO student(id,name,sex)VALUES(2,'wang','male');
>>>已创建 1 行。<br>

#### 创建视图并查询
>创建视图<br>
>>  CREATE VIEW studentview AS SELECT name FROM student;<br>
>>> 视图已创建<br>
>查询视图<br>
>>SELECT * FROM myview;<br>
>>>NAME<br>
>>>--------------------------------------------------------------------------------<br>
>>>zhang<br>
>>>liu<br>
>>>wang<br>

### 查看数据库的使用情况
#### SQL> show user;
>USER 为 "SYSTEM"<br>

>SELECT tablespace_name,FILE_NAME,BYTES/1024/1024 MB,MAXBYTES/1024/1024 MAX_MB,autoextensible FROM dba_data_files  WHERE  >tablespace_name='USERS';

>>TABLESPACE_NAME
>>--------------------------------------------------------------------------------
>>FILE_NAME
>>--------------------------------------------------------------------------------
>>        MB     MAX_MB AUTOEXTEN
>>---------- ---------- ---------
>>USERS
>>/home/oracle/app/oracle/oradata/orcl/pdborcl/SAMPLE_SCHEMA_users01.dbf
>>        5 32767.9844 YES
         
>SELECT a.tablespace_name "表空间名",Total/1024/1024 "大小MB", <br>
>free/1024/1024 "剩余MB",( total - free )/1024/1024 "使用MB", <br>
> Round(( total - free )/ total,4)* 100 "使用率%" <br>
> from (SELECT tablespace_name,Sum(bytes)free <br>
>        FROM   dba_free_space group  BY tablespace_name)a, <br>
>       (SELECT tablespace_name,Sum(bytes)total FROM dba_data_files <br>
>        group  BY tablespace_name)b <br>
> where  a.tablespace_name = b.tablespace_name; <br>
>> 表空间名  <br>
>>--------------------------------------------------------------------------------  <br>
>>    大小MB     剩余MB     使用MB    使用率%  <br>
>>---------- ---------- ---------- ----------  <br>
>>SYSAUX  <br>
>>       630      35.75     594.25      94.33  <br>
>>  <br>
>>USERS  <br>
>>         5        .25       4.75         95  <br>
>>  <br>
>>SYSTEM  <br>
>>       270     3.5625   266.4375      98.68  <br>
>>  <br>
>>表空间名  <br>
>>--------------------------------------------------------------------------------  <br>
>>   大小MB     剩余MB     使用MB    使用率%  <br>
>>---------- ---------- ---------- ----------  <br>
>>EXAMPLE  <br>
>>  1281.875      62.25   1219.625      95.14  <br>

