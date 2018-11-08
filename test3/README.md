
# 实验3：创建分区表

时间: 2018年10月31日

## 实验目的：

掌握分区表的创建方法，掌握各种分区方式的使用场景。

## 实验内容：

### 1.本实验使用3个表空间：USERS,USERS02,USERS03。在表空间中创建两张表：订单表(orders)与订单详表(order_details)。
### - 使用**你自己的账号创建本实验的表**，表创建在上述3个分区，自定义分区策略。
#### 三个分区分别是users users02 users03
 <font color='red'>*** 创建orders表 </font>         
```
SQL> CREATE TABLE orders                                   
(                                       
 order_id NUMBER(10, 0) NOT NULL                                        
 , customer_name VARCHAR2(40 BYTE) NOT NULL                                      
 , customer_tel VARCHAR2(40 BYTE) NOT NULL                                 
 , order_date DATE NOT NULL                                     
 , employee_id NUMBER(6, 0) NOT NULL                                 
 , discount NUMBER(8, 2) DEFAULT 0                                       
 , trade_receivable NUMBER(8, 2) DEFAULT 0                                     
)                                       
TABLESPACE USERS                                       
PCTFREE 10 INITRANS 1                                       
STORAGE (   BUFFER_POOL DEFAULT )                                 
NOCOMPRESS NOPARALLEL                                
PARTITION BY RANGE (order_date)                                  
(                                       
 PARTITION PARTITION_BEFORE_2016 VALUES LESS THAN (                            
 TO_DATE(' 2016-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS',                                   
 'NLS_CALENDAR=GREGORIAN'))                                     
 NOLOGGING                                       
 TABLESPACE USERS                                     
 PCTFREE 10                                     
 INITRANS 1                                    
 STORAGE                                    
(                                   
 INITIAL 8388608                                      
 NEXT 1048576                                    
 MINEXTENTS 1                                     
 MAXEXTENTS UNLIMITED                                     
 BUFFER_POOL DEFAULT                                      
)                                      
NOCOMPRESS NO INMEMORY                                       
, PARTITION PARTITION_BEFORE_2017 VALUES LESS THAN (                                   
TO_DATE(' 2017-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS',                                
'NLS_CALENDAR=GREGORIAN'))                                      
NOLOGGING                                     
TABLESPACE USERS02                                      
PCTFREE 10                                     
 INITRANS 1                                       
 STORAGE                                     
(                                  
 INITIAL 8388608                          
 NEXT 1048576                           
 MINEXTENTS 1                            
 MAXEXTENTS UNLIMITED                  
 BUFFER_POOL DEFAULT                    
)                                 
NOCOMPRESS NO INMEMORY                       
, PARTITION PARTITION_BEFORE_2018 VALUES LESS THAN (                 
TO_DATE(' 2018-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS',                     
'NLS_CALENDAR=GREGORIAN'))                            
NOLOGGING                            
TABLESPACE USERS03                    
PCTFREE 10                       
 INITRANS 1                        
 STORAGE                        
(                                
 INITIAL 8388608                       
 NEXT 1048576                                
 MINEXTENTS 1                                 
 MAXEXTENTS UNLIMITED             
 BUFFER_POOL DEFAULT                   
)                           
);                             
```
<font color='red'>创建表(order_details)。</font>   
```
CREATE TABLE order_details                 
(                                
id NUMBER(10, 0) NOT NULL                                 
, order_id NUMBER(10, 0) NOT NULL                         
, product_id VARCHAR2(40 BYTE) NOT NULL                            
, product_num NUMBER(8, 2) NOT NULL                            
, product_price NUMBER(8, 2) NOT NULL                             
, CONSTRAINT order_details_fk1 FOREIGN KEY  (order_id)                 
REFERENCES orders  (  order_id   )                            
ENABLE                                 
)                        
TABLESPACE USERS                         
PCTFREE 10 INITRANS 1                                 
STORAGE (   BUFFER_POOL DEFAULT )                            
NOCOMPRESS NOPARALLEL                               
PARTITION BY REFERENCE (order_details_fk1)                              
(                                 
PARTITION PARTITION_BEFORE_2016                                   
NOLOGGING                                   
TABLESPACE USERS                                     
PCTFREE 10                               
 INITRANS 1                                    
 STORAGE                                     
(                                     
 INITIAL 8388608                                  
 NEXT 1048576                              
 MINEXTENTS 1                              
 MAXEXTENTS UNLIMITED                               
 BUFFER_POOL DEFAULT                            
)                                 
)                                
NOCOMPRESS NO INMEMORY,                                  
PARTITION PARTITION_BEFORE_2017                               
NOLOGGING                                    
TABLESPACE USERS02                             
PCTFREE 10                                
 INITRANS 1                                    
 STORAGE                                     
(                                    
 INITIAL 8388608                                  
 NEXT 1048576                                 
 MINEXTENTS 1                               
 MAXEXTENTS UNLIMITED                                 
 BUFFER_POOL DEFAULT                                     
)                                     
)                                       
NOCOMPRESS NO INMEMORY,                                   
PARTITION PARTITION_BEFORE_2018                                
NOLOGGING                                  
TABLESPACE USERS03                                   
PCTFREE 10                                   
 INITRANS 1                                   
 STORAGE                               
(                                     
 INITIAL 8388608                                     
 NEXT 1048576                                       
 MINEXTENTS 1                                      
 MAXEXTENTS UNLIMITED                                      
 BUFFER_POOL DEFAULT                                      
)                                      
);                                      
```
### 你需要使用system用户给你自己的账号分配上述分区的使用权限。你需要使用system用户给你的用户分配可以查询执行计划的权限。

![Image text](https://github.com/thefoxsayold/oracle/blob/master/test3/QQ%E6%88%AA%E5%9B%BE20181031210107.png)

### 表创建成功后，插入数据，数据能并平均分布到各个分区。每个表的数据都应该大于1万行，对表进行联合查询。
#### 插入orders
``` 
declare
i integer; 
begin
    i := 1;
    loop
        if i<=3000
        then
            insert into NEW_ZB.ORDERS (order_id, customer_name, customer_tel, order_date, employee_id, discount, trade_receivable)
            values(i,'zzc','zzc',to_date('2015-02-14','yyyy-mm-dd'),i,i,i);
        
        elsif i<=5000
        then
            insert into NEW_ZB.ORDERS (order_id, customer_name, customer_tel, order_date, employee_id, discount, trade_receivable)
                values(i,'zzc','zzc',to_date('2016-02-14','yyyy-mm-dd'),i,i,i);
        
        elsif i<=7000
        then
            insert into NEW_ZB.ORDERS (order_id, customer_name, customer_tel, order_date, employee_id, discount, trade_receivable)
                values(i,'zzc','zzc',to_date('2016-02-14','yyyy-mm-dd'),i,i,i);
        
        else 
            insert into NEW_ZB.ORDERS (order_id, customer_name, customer_tel, order_date, employee_id, discount, trade_receivable)
                values(i,'zzc','zzc',to_date('2017-1-1','yyyy-mm-dd'),i,i,i);
        end if;
        i := i + 1;
        /* 停止条件 */
        exit when i > 12000;
    end loop;
    commit;
end;
```
#### 插入 order_detalis
```
declare
i integer; 
begin
    i := 1;
    loop
        if i<=3000
        then
            insert into NEW_ZB.ORDER_DETAILS (ID, ORDER_ID, PRODUCT_ID, PRODUCT_NUM, PRODUCT_PRICE)
            values(i,i,'zzb',500,50);
        
        elsif i<=5000
        then
            insert into NEW_ZB.ORDER_DETAILS (ID, ORDER_ID, PRODUCT_ID, PRODUCT_NUM, PRODUCT_PRICE)
            values(i,i,'zzb',400,70);
        
        elsif i<=7000
        then
            insert into NEW_ZB.ORDER_DETAILS (ID, ORDER_ID, PRODUCT_ID, PRODUCT_NUM, PRODUCT_PRICE)
            values(i,i,'zzb',300,10);
        
        else 
             insert into NEW_ZB.ORDER_DETAILS (ID, ORDER_ID, PRODUCT_ID, PRODUCT_NUM, PRODUCT_PRICE)
            values(i,i,'zzb',800,200);
        end if;
        i := i + 1;
        /* 停止条件 */
        exit when i > 12000;
    end loop;
    commit;
end;
```

### 写出插入数据的语句和查询数据的语句，并分析语句的执行计划。

```
select e.ORDER_ID,e.CUSTOMER_NAME,e.ORDER_DATE,d.PRODUCT_ID,d.PRODUCT_NUM,d.PRODUCT_PRICE
from ORDERS e,ORDER_DETAILS d
where  e.ORDER_ID=d.ORDER_ID
and e.ORDER_DATE> to_date('2016-12-07 08:00:00','yyyy-mm-dd hh24:mi:ss');

```

### 进行分区与不分区的对比实验。

![Image text](https://github.com/thefoxsayold/oracle/blob/master/test3/%E6%8D%95%E8%8E%B7.PNG)

分区后查找效率更高

