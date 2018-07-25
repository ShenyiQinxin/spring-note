# Fetch the count of employees working in project 'P1'.
```sql
select count(*) from employeeSalary where project = 'P1';
```

# Fetch employee names having salary greater than or equal to 5000 and less than or equal 10000.
```sql

select fullname from EmployeeDetails where empid in 
(select empid from employeeSalary where salary between 5000 and 10000);

select fullname from EmployeeDetails ed inner join EmployeeSalary es using empid
where employeeSalary between 5000 and 10000;
```

# Fetch project-wise count of employees sorted by project's count in descending order
```sql
select project, count(empid) project_count from employeeSalary
group by project
order by project_count;
```

# Fetch only the first name(string before space) from the FullName column of EmployeeDetails table.
```sql
select substr(fullname, 1, instr(fullname,' ')) from EmployeeDetails
```

# fetch employee names and salary records. Return employee details even if the salary record is not present for the employee.
```sql
select es.salary, ed.fullname
from employeeSalary es left out join EmployeeDetails ed
using empid
```
# fetch all the Employees who are also managers from EmployeeDetails table
```sql
select e.empid
from EmployeeDetails e inner join EmployeeDetails m on e.managerid= m.empid
```

# Fetch all employee records from EmployeeDetails table who have a salary record in EmployeeSalary table.
```sql
select * from EmployeeDetails where empid in (select empid from employeeSalary where employeeSalary!=null)

select * from EmployeeDetails e where exists (select * from employeeSalary s where e.empid = s.empid);

```

# fetch duplicate records from a table.
```sql
select * from EmployeeSalary where rowid not in (select min(rowid) from EmployeeSalary group by empid)
```

# remove duplicates from a table without using temporary table
```sql
delete from employeeSalary where empid in (select empid from employeeSalary
group by empid having count(empid)>1); 
```

# fetch only odd rows from table.
```sql
select * from (select employeeSalary.*,rowmun rn from employeeSalary)
where mod(rn,2)==1
```
# create a new table with data and structure copied from another table
```sql
create table new_table as 
	select * from employeeSalary;
```

# create an empty table with same structure as some other table.
```sql
create table new_table as select * from employeeSalary where 1=2;
```

# fetch common records between two tables.
```sql
select * from employeeSalary1
intersect
select * from employeeSalary2
```
# fetch records that are present in one table but not in another table.
```sql
select * from employeeSalary1
minus 
select * from employeeSalary2
```

# query to find current date-time.
```sql
select sysdate from dual;
```
# query to fetch all the Employees from EmployeeDetails table who joined in Year 2016.
```sql
select * from EmployeeDetails where dateofJoining '1/1/1980' and '1/1/1982'
```
# fetch top n records.
```sql
//no sorted
select * from emp where rownum<=8
//sorted
select * from (select * from employeeSalary order by salary desc) 
where rownum<=3;
```
# Find the nth highest salary from table.
```sql
select * from (select emp.*, dense_rank(over salary desc) rn from emp) where rn<=9
```

# find the 3rd highest salary from table without using TOP/limit keyword
```sql
select * from emp e1 where 2 = (select count(distinct(salary)) 
	from emp e2 where e2.salary>e1.salary);
```
> ## table1: EmployeeSalary(empid, project, salary)

> ## table2: EmployeeDetails(empid, fullname, managerid, dateofJoining)



# 2.1 The second highest salary among all Employees
```sql
select max(sal)
from emp
where sal != (select max(sal) from emp);
```
# 2.2 Select 5th maximum salary from a table
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
where e2.sal > e1.sal
)
```
- with function `dense_rank() ` continuously ranking
```sql
select * from 
(select emp.*, dense_rank() over (order by nvl(sal,0) desc) rn from emp)
where rn=5
```
##  3 find the maximum salary from the EMP table without using functions.
```sql
select * from emp
where sal not in
(
select e1.sal from emp e1 inner join emp e2 on e1.sal<e2.sal
)
```
## 4 Top N salaries from EMP table
>select maximum N salaries from the EMP table
```sql
select * from 
(
select emp.*, dense_rank() over (order by sal desc) rn from emp 
)
where rn<=5;
```
## 5 Select top 3 salaries from each Department of EMP table
> select maximum N salaries from each department of the EMP table
```sql
select * from
(
select emp.*, dense_rank() over (partition by deptno order by sal desc) rn
from emp
)
where rn<=3
```
## 10 Select LAST 7 records from a table
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
# dup
##  6 select/delete duplicate rows from the EMP table
> rowid
> records with min(rowid) are  a unique set, others are dups
```sql
select * from emp
where rowid not in
(
select min(rowid) from emp group by empno
)

delete from emp
where rowid not in
(select min(rowid) from emp group by empno

select empno, ename, count(*)
from emp 
group by empno, ename
having count(*)>1
```
## 7 select only those employee information who are earning the same salary?
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

## 8 Display the employees where "more than 2 employees under a manager"
```sql
select * from 
(select emp.*, count(empno) over (partition by mgr) as cnt from emp)
where cnt>=2

//manager with its employee count
select e1.empno, e1.mgr, e2.cnt
from emp e1 inner join (
select mgr, count(empno) cnt
from emp
group by mgr
having count(empno)>=2
order by cnt
) e2
on e1.mgr = e2.mgr

```
## 9 display even/odd number rows from a table
//first retrieve the rownum as a column RN
//if directly filter by mod(rownum,2), then it only retrieves the 1st row found
```sql
select * from
(select empno, ename, sal, rownum rn from emp order by empno)
where mod(rn, 2)!=0
order by rn
```
##  11 find the employees who are working in the company for the past 5 years.
```sql
select * from emp where hiredate<= add_months(sysdate, -60);
```
##  12 find the LAST inserted record in a table.
>If you want the last record inserted, you need to have a timestamp or sequence number assigned to each
record as they are inserted and then you can use the below query…
```sql
//assume empno is increased and assigned to a new employee when he joins in
select * from emp
where empno = (
select max(empno) from emp
)
```
## max salary n dept name from each department
```sql
select d.dname, nvl(max(e.sal),0) salary
from emp e full outer join dept d
on e.deptno = d.deptno
group by d.dname
```
## the nth highest salary among all Employees
```sql
select * from 
(select emp.*, row_number() over (order by sal desc) rn from emp) where rn<=7
```
```sql
select * from 
(select emp.*, dense_rank() over (order by sal desc) rn from emp) where rn<=7
```
## find top10 employees with Odd number as Employee ID
```sql
select empno from emp where mod(empno,2)=1 and rownum<=10
```
## get the Quarter from date
```sql
select to_char(to_date('12/31/2016', 'MM/DD/YYYY'), 'Q') as quarter from dual;
select to_char(to_date('12/31/2016', 'MM/DD/YYYY'), 'MONTH') as month from dual;
select to_char(to_date('12/31/2016', 'MM/DD/YYYY'), 'YEAR') as year from dual;
select to_char(to_date('12/31/2016', 'MM/DD/YYYY'), 'DAY') as day from dual;
```
## find all employee whose name contains the word "Rich", regardless of case
```sql
select * from emp where lower(ename) like '%ing%'
```
## print a comma separated list of emp names , listed in the order of hiredate, group them by the sal
```sql
select sal, listagg(ename, ',') within group (order by hiredate) as emps
from emp
group by sal
```
###  1 the number of rows in a table without using COUNT function.
```sql
select max(rownum ) from emp;
```