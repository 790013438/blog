配置和移植工具
监听程序配置
删除

添加
tcp
默认
配一个监听

```sql
select name from emp where sal in (select sal from emp where sal >= max(sal))
```
* 查询最大的月薪
    ```sql
    select name from emp where sal =(select max(sal) from emp where)
    ```
* 查询年薪和补贴
    ```sql
    select name, (sal+nvl(comm,0))*16 from emp
    ```
    * 降序配列
    ```sql
    select ename, (sal+nvl(comm,0))*16 as total from emp order by total desc;
    ```
* 查询每位员工的姓名和年薪(10部门13薪，20部门16薪，其他12薪)
    ```sql
    select name from emp where 
    select deptno from emp
    ```
    ```sql
    select deptno, count(deptno) from emp group by depton;
    ```
    * 正确版
    ```sql
        select dname,total from 
        (select deptno,count(deptno) as total from emp 
        group by deptno) t1 
        inner join dept 12 on t1.deptno=t2.deptno;
    ```
    * 查询超过4个人的部门名称和人数
    ```sql
    select dname,total from 
    (select deptno,count(deptno) as total from emp 
    group by deptno) having count(deptno) > 4 t1
    inner join dept 12 on t1.deptno=t2.deptno;
    ```
    * 查询所有部门的名称和人数
    ```sql
    select dname,nvl(total,0) as total from 
    (select deptno,count(deptno) as total from emp 
    group by deptno)  t1
    right out join dept t2 on t1.deptno=t2.deptno;
    ```
    * 右外连接
    ```sql
    select dname,nvl(total,0) as total from 
    (select deptno,count(deptno)as total from emp group by deptno)t1, dept t2 
    where t1.deptno(+)=t2.deptno;
    ```
    * 全连接，笛卡尔积
        > 用的比较少
    ```sql
    select t1.*, t2.* from dept t1, emp t2;
    ```
        > 相当左外连接
        ```sql
        select t1.*, t2.* from dept t1, emp t2;
        where t1.deptno=t2.deptno;
        ```
        > 相当圈外谅解
        ```sql
        select t1.dname, t2.ename from dept t1 full join emp t2 on 1=1;
        ```
    * 自然连接（用外键连接）
        > 右表越小越好
    ```sql
    select t1.*, t2.* from emp t1, dept t2 where t1.deptno=t2.deptno;
    ```
    ```sql
    select * from emp natural join dept;
    ```
    * 查询薪水最高的前5名雇员
        > rownum 伪列
    ```sql
    select rownum, ename, sal from 
    (select ename, sal from emp 
    order by sal desc) t1 where rownum <= 5
    ```
* 查询薪水排在第6-10名雇员
```sql
select ename, sal from
(select rownum as rn, t1.* from
(select ename, sal from emp
order by sal desc) t1 ) t2
where rn between 6 and 10;
```
* 查询月薪排名前5的员工姓名和工资
```sql
select ename, sal from 
(select rownum as rn, ename, sal 
from (select ename, sal from Emp order by sal desc) t1 ) t2
where rn <= 5
```
* 查询月薪排名前5的员工姓名和工资
```sql
select ename, sal from 
(select rownum as rn, ename, sal 
from (select ename, sal from Emp order by sal desc) t1 ) t2
where rn between 4 and 8;
```
* 查询薪水超过平均薪水的员工的姓名和工资
```sql
select ename, sal from Emp 
where sal>(select avg(sal) from Emp)
```
    > 补平均工资的列 
    ```sql
    select ename, sal,(select avg(sal) from Empa) as avgSal from Emp 
    where sal>(select avg(sal) from Emp)
    ```
* 查询薪水超过其所在部门平均薪水的员工的姓名，部门编号和工资
```sql
select ename, t1.deptno, sal, avgSal from Emp t1 inner join
(select deptno, avg(sal) as avgSal from Emp group by deptno) t2
on t1.deptno=t2.deptno and sal>avgSal
```
    * 查询薪水超过其所在部门平均薪水的员工的姓名，部门名称和工资
    ```sql
    select ename, dname, sal from 
    (select ename, t1.deptno, sal from Emp t1 inner join
    (select deptno, avg(sal) as avgSal from Emp group by deptno) t2
    on t1.deptno=t2.deptno and sal>avgSal) t3 inner join
    Dept t4 on t3.deptno=t4.deptno;
    ```
        > 小表放后，做驱动表
* 查询主管的姓名和职位
    * 查询主管的编号
    * 去重```distinct```
```sql
select ename, job from Emp 
where empno in (select distinct mgr from Emp where mgr is not null);
```
    * 去重,一般使用exists / no exists 去重
o   ```sql
    select 'x' from Emp
    ```
        > 查询有没有
    * 查询主管 更好的写法
    ```sql
    select ename,job from Emp t1 
    where exists (select 'x' from Emp t2 where t1.empno = t2.mgr)
    ```
* 平均薪水最高的部门的名称和平均工资
```sql
select dname, avgSal from Dept t3
inner join 
(select t2.deptno, avgSal from 
(select deptno, avg(sal) as avgSal from Emp group by deptno) t2
where avgSal=
(select max(avgSal) as maxSal from 
(select deptno, avg(sal) as avgSal from Emp group by deptno) t1)) t4
on t4.deptno=t3.deptno;
```
    * having, 更简单的
    ```sql
    select dname, avgSal from 
        (select deptno, avgSal from 
            (select deptno, avg(sal)as avgSal 
                from Emp group by deptno
                having avg(sal)=(
                    select max(avgSal) as maxSal from 
                    (select deptno, avg(sal) as avgSal from Emp group by deptno) t1
                )) t2 
            inner join Dept t3 
            on t2.deptno=t3.deptno
    ```
