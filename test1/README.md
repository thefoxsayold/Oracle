# 实验一：分析SQL执行计划，执行SQL语句的优化指导
时间：2018年10月17日。 
## 实验内容：
- 对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。
- 首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。


## 教材中的查询语句

查询1：

```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```

查询一结果：
![Image text](https://github.com/thefoxsayold/oracle/blob/master/test1/%E6%9F%A5%E8%AF%A21.png)


- 查询2：
```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```

查询二结果

![Image text](https://github.com/thefoxsayold/oracle/blob/master/test1/%E6%9F%A5%E8%AF%A22.png)

## 两个的详细情况进行对比分析

查询一详情

![Image text](https://github.com/thefoxsayold/oracle/blob/master/test1/%E6%9F%A5%E8%AF%A2%E4%B8%80%E8%AF%A6%E6%83%85.png)

查询二详情

![Image text](https://github.com/thefoxsayold/oracle/blob/master/test1/%E6%9F%A5%E8%AF%A2%E4%BA%8C%E8%AF%A6%E6%83%85.png)

## 分析对比

**在查询语句开头加 SET AUTOTRACE ON 运行两次现实详细的情况对比两次的查询语句的消耗。**
consistent gets:从buffer cache 中读取的undo数据的block数量，physical reads和consistent gets是我们最关心的，如果physical reads很高，说明要从磁盘请求大量的数据到buffer cache；里，通常意味着系统里存在大量的全表扫描的SQL语句，这会影响到数据库的性能。
所以从数据上来看第一个查询语句消耗要少一点更优一些。



- 设计自己的查询语句，并作相应的分析，查询语句不能太简单。
