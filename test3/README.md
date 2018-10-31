
# 实验3：创建分区表

时间: 2018年10月31日

## 实验目的：

掌握分区表的创建方法，掌握各种分区方式的使用场景。

## 实验内容：

### 本实验使用3个表空间：USERS,USERS02,USERS03。在表空间中创建两张表：订单表(orders)与订单详表(order_details)。
### - 使用**你自己的账号创建本实验的表**，表创建在上述3个分区，自定义分区策略。
SQL> CREATE TABLE orders    <font color='red'>创建orders表 </font>                                         <br>
(                                       <br>
 order_id NUMBER(10, 0) NOT NULL                                        <br>
 , customer_name VARCHAR2(40 BYTE) NOT NULL                                        <br>
 , customer_tel VARCHAR2(40 BYTE) NOT NULL                                        <br>
 , order_date DATE NOT NULL                                        <br>
 , employee_id NUMBER(6, 0) NOT NULL                                        <br>
 , discount NUMBER(8, 2) DEFAULT 0                                        <br>
 , trade_receivable NUMBER(8, 2) DEFAULT 0                                        <br>
)                                        <br>
TABLESPACE USERS                                        <br>
PCTFREE 10 INITRANS 1                                        <br>
STORAGE (   BUFFER_POOL DEFAULT )                                        <br>
NOCOMPRESS NOPARALLEL                                        <br>
PARTITION BY RANGE (order_date)                                        <br>
(                                       <br>
 PARTITION PARTITION_BEFORE_2016 VALUES LESS THAN (                                       <br>
 TO_DATE(' 2016-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS',                                        <br>
 'NLS_CALENDAR=GREGORIAN'))                                        <br>
 NOLOGGING                                        <br>
 TABLESPACE USERS                                        <br>
 PCTFREE 10                                        <br>
 INITRANS 1                                        <br>
 STORAGE                                        <br>
(                                        <br>
 INITIAL 8388608                                        <br>
 NEXT 1048576                                        <br>
 MINEXTENTS 1                                        <br>
 MAXEXTENTS UNLIMITED                                        <br>
 BUFFER_POOL DEFAULT                                        <br>
)                                        <br>
NOCOMPRESS NO INMEMORY                                         <br>
, PARTITION PARTITION_BEFORE_2017 VALUES LESS THAN (                                       <br>
TO_DATE(' 2017-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS',                                        <br>
'NLS_CALENDAR=GREGORIAN'))                                        <br>
NOLOGGING                                        <br>
TABLESPACE USERS02                                        <br>
PCTFREE 10                                        <br>
 INITRANS 1                                        <br>
 STORAGE                                        <br>
(                                        <br>
 INITIAL 8388608                                        <br>
 NEXT 1048576                                        <br>
 MINEXTENTS 1                                        <br>
 MAXEXTENTS UNLIMITED                                        <br>
 BUFFER_POOL DEFAULT                                        <br>
)                                        <br>
NOCOMPRESS NO INMEMORY                                         <br>
, PARTITION PARTITION_BEFORE_2018 VALUES LESS THAN (                                       <br>
TO_DATE(' 2018-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS',                                        <br>
'NLS_CALENDAR=GREGORIAN'))                                        <br>
NOLOGGING                                        <br>
TABLESPACE USERS03                                       <br>
PCTFREE 10                                        <br>
 INITRANS 1                                        <br>
 STORAGE                                        <br>
(                                        <br>
 INITIAL 8388608                                        <br>
 NEXT 1048576                                        <br>
 MINEXTENTS 1                                        <br>
 MAXEXTENTS UNLIMITED                                        <br>
 BUFFER_POOL DEFAULT                                        <br>
)                                        <br>
);                                       <br>


CREATE TABLE order_details   <font color='red'>创建表(order_details)。</font>                                      <br>
(                                       <br>
id NUMBER(10, 0) NOT NULL                                        <br>
, order_id NUMBER(10, 0) NOT NULL                                       <br>
, product_id VARCHAR2(40 BYTE) NOT NULL                                        <br>
, product_num NUMBER(8, 2) NOT NULL                                        <br>
, product_price NUMBER(8, 2) NOT NULL                                        <br>
, CONSTRAINT order_details_fk1 FOREIGN KEY  (order_id)                                       <br>
REFERENCES orders  (  order_id   )                                       <br>
ENABLE                                        <br>
)                                        <br>
TABLESPACE USERS                                        <br>
PCTFREE 10 INITRANS 1                                        <br>
STORAGE (   BUFFER_POOL DEFAULT )                                        <br>
NOCOMPRESS NOPARALLEL                                       <br>
PARTITION BY REFERENCE (order_details_fk1)                                       <br>
(                                       <br>
PARTITION PARTITION_BEFORE_2016                                        <br>
NOLOGGING                                        <br>
TABLESPACE USERS                                       <br>
PCTFREE 10                                        <br>
 INITRANS 1                                        <br>
 STORAGE                                        <br>
(                                        <br>
 INITIAL 8388608                                        <br>
 NEXT 1048576                                        <br>
 MINEXTENTS 1                                        <br>
 MAXEXTENTS UNLIMITED                                        <br>
 BUFFER_POOL DEFAULT                                        <br>
)                                        <br>
)                                        <br>
NOCOMPRESS NO INMEMORY,                                        <br>
PARTITION PARTITION_BEFORE_2017                                        <br>
NOLOGGING                                        <br>
TABLESPACE USERS02                                       <br>
PCTFREE 10                                        <br>
 INITRANS 1                                        <br>
 STORAGE                                        <br>
(                                        <br>
 INITIAL 8388608                                        <br>
 NEXT 1048576                                        <br>
 MINEXTENTS 1                                        <br>
 MAXEXTENTS UNLIMITED                                        <br>
 BUFFER_POOL DEFAULT                                        <br>
)                                        <br>
)                                        <br>
NOCOMPRESS NO INMEMORY,                                       <br>
PARTITION PARTITION_BEFORE_2018                                       <br>
NOLOGGING                                        <br>
TABLESPACE USERS03                                       <br>
PCTFREE 10                                        <br>
 INITRANS 1                                        <br>
 STORAGE                                        <br>
(                                        <br>
 INITIAL 8388608                                        <br>
 NEXT 1048576                                        <br>
 MINEXTENTS 1                                        <br>
 MAXEXTENTS UNLIMITED                                        <br>
 BUFFER_POOL DEFAULT                                        <br>
)                                        <br>
);                                       <br>

###
