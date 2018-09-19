# Oracle 数据库应用

## 内容说明

- PPT目录：各章节的PPT
- script目录：各章节的SQL语句及源码
- doc目录：参考文档
- book.pdf： 本书的PDF文档

## CDB切换到PDB过程
S
$sqlplus / as sysdba  `SYS:CDB$ROOT`
SQL>show user `SYS`
SQL>show con_name  `CDB$ROOT`
SQL>show pdbs  `PDB$SEED, pdborcl ,…`
SQL>ALTER SESSION SET CONTAINER=pdborcl;  `CDB切换到PDBORCL`
SQL>show con_name  `PDBORCL`
SQL>DESC all_users; `查询表的字段+属性`
SQL>select username from all_users; `HR,SCOTT,SH,...`
SQL>desc all_tables; `table_name,owner` 
SQL>select table_name,owner from all_tables where owner=’HR’; `hr拥有的所有表名称`
SQL>select * from hr.jobs; `SYS用户访问HR用户的表JOBS`


## 其他
```
$sqlplus hr/123@localhost:1521/pdborcl:dedicated
$lsnrctl service 连接数量在增加
$ps –ef | grep oracleorcl 专用进程在增加
```

## 网址
- Oracle Database 12c Release 2 下载地址:

    http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html

- SQL Developer 客户端工具下载地址:

    https://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html