## Select 5th maximum salary from a table
>Write a query to select Nth maximum salary from EMP table
(or)
Write a query to find 2nd, 3rd max salary from EMP table
(or)
Write a query to find 10 highest salary
(Or)
Write a query to find 4th highest salary (without analytical function)
- without function
```sql
select * 
from emp e1
where (5-1) =
(select count(distinct(e2.sal))
from emp e2
where e2.sal >= e1.sal
)
```
- with function `dense_rank() ` continuously ranking
```sql
select * from 
(select emp.*, dense_rank() over (order by sal desc) rn from emp)
where rn=5
```
## Top N salaries from EMP table
>select maximum N salaries from the EMP table
```sql
select * from 
(
select emp.*, dense_rank() over (order by nvl(sal,0) desc) rn from emp 
)
where rn<=5;
```
### Select top 3 salaries from each Department of EMP table
> select maximum N salaries from each department of the EMP table
```sql
select * from
(
select emp.*, dense_rank() over (partition by deptno order by sal desc) rn
from emp
)
where rn<=3
```
##  select/delete duplicate rows from the EMP table
> rowid
```sql
select * from emp
where rowid not in
(
select min(rowid) from emp group by empno
)

delete from emp
where rowid not in
(select min(rowid) from emp group by empno
```
## select only those employee information who are earning the same salary?
- 1st
```sql
select e1.* from emp e1 inner join emp e2 
on e1.sal=e2.sal and e1.ename!=e2.ename
```
- 2nd
```sql
select * from emp where sal in
(
select sal from emp group by sal having count(sal)>=2
)
```
- 3rd
```sql
select * from 
(select emp.*, count(*) over (partition by sal orderby sal) cnt from emp)
where cnt>=2;
```
## display even/odd number rows from a table
```sql
select * from
(select empno, ename, sal, rownum rn from emp order by empno)
where mod(rn, 2)!=0
order by rn
```
## more than 2 employees under a manager
```sql
select * from 
(select *, count(mgr) over (partition by mgr) as cnt from emp)
where cnt>=2
```
##  find the maximum salary from the EMP table without using functions.
```sql
select * from emp
where sal not in
(
select e1.sal from emp e1 inner join emp e2 on e1.sal<e2.sal
)
```
##  the number of rows in a table without using COUNT function.
```sql
select max(rownum ) from 
(select rownum from emp )
```
##  find the LAST inserted record in a table.
>If you want the last record inserted, you need to have a timestamp or sequence number assigned to each
record as they are inserted and then you can use the below query…
```sql
select * from emp
where assume_rowid= (select max(assume_rowid) from emp);
```
## Select LAST 7 records from a table
```sql
//15 rows in total
//last 7 rows
select emp.*, rownum, rowid from emp 
minus
select emp.*, rownum, rowid from emp 
where rownum<=(select count(*)-7 from emp)
//first 8 rows
select emp.*, rownum, rowid from emp 
where rownum<=8;
```
##  find the employees who are working in the company for the past 5 years.
```sql
select * from emp where hiredate< add_months(sysdate, -60);
```
##
```sql
```
##
```sql
```
##
```sql
```
##
```sql
```
##
```sql
```
