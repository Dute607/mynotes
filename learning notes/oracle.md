#### 多表查询
* 内连接
* 外连接
  > 外连接是将多表查询时不符合条件的数据也展示出来，(+)在哪边，哪边就没有数据
  * 左外连接
```sql
select a.lase_name,a.department_id,b.department_name
from employees a,departments b
where a.department_id = b.department_id(+);
```
  * 右外连接
```sql
select a.lase_name,a.department_id,b.department_name
from employees a,departments b
where a.department_id(+) = b.department_id;
```
