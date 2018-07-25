
> ## table1: EmployeeSalary(empid, project, salary)

> ## table2: EmployeeDetails(empid, fullname, managerid, dateofJoining)

### 1. Fetch the count of employees working in project 'P1'.
```sql
select count(*) from EmployeeSalary where project='P1'
```

### 2. Fetch employee names having salary greater than or equal to 5000 and less than or equal 10000.
```sql
select ed.fullname from EmployeeSalary es join EmployeeDetails ed on es.empid=ed.empid 
where es.salary between 5000 and 1000
```
### 3. Fetch project-wise count of employees sorted by project's count in descending order.
```sql
select project, count(empid) emp_count_per_project from EmployeeSalary 
group by project 
sorted by emp_count_per_project desc;
```
### 4. Fetch only the first name(string before space) from the FullName column of EmployeeDetails table.
```sql
select substr(fullname, 1, instr(fullname, ' ')-1) first_name from EmployeeDetails;
```
### 5. fetch employee names and salary records. Return employee details even if the salary record is not present for the employee.
```sql
select ed.fullname, es.salary 
from EmployeeSalary es join EmployeeDetails ed 
on es.empid = ed.empid
where 
```

### 6. fetch all the Employees who are also managers from EmployeeDetails table.
```sql
```


### 7. Fetch all employee records from EmployeeDetails table who have a salary record in EmployeeSalary table.
```sql
```


### 8. fetch duplicate records from a table.
```sql
```


### 9. remove duplicates from a table without using temporary table.
```sql
```


### 10. fetch only odd rows from table.
```sql
```

### 11 create a new table with data and structure copied from another table.
```sql
```

### 12 create an empty table with same structure as some other table.
```sql
```

### 13 fetch common records between two tables.
```sql
```

### 14 fetch records that are present in one table but not in another table.
### 15 query to find current date-time.
### 16 query to fetch all the Employees from EmployeeDetails table who joined in Year 2016.
### 17 fetch top n records.
### 18 Find the nth highest salary from table.
### 19 find the 3rd highest salary from table without using TOP/limit keyword.


```sql
--1
select count(*) from EmployeeSalary where project='P1';

--2
select fullname from EmployeeDetails ed inner join EmployeeSalary es on ed.empid=ed.empid
where es.salary between 5000 and 10000;

--3
select project, count(empid) emp_count_per_project from EmployeeSalary group by project order by emp_count_per_project desc;

--4
select substr(fullname, 1, instr(fullname, ' ')-1) from EmployeeDetails;

--5
select ed.fullname, es.salary from EmployeeSalary es right inner join EmployeeDetails ed on es.empid = ed.empid

--6
select e.* from EmployeeDetails e inner join EmployeeDetails m on e.empid = m.managerid; 

--7
select * from EmployeeDetails d where exists (select * from EmployeeSalary e where d.emid=e.empid)

--8
select * from EmployeeSalary where rowid not in (select min(rowid) from EmployeeSalary group by empid)

--9 
delete from EmployeeSalary where rowid not in (select min(rowid) from EmployeeSalary group by empid);

--11
create table table1 as select * from EmployeeSalary

--10
select * from  EmployeeSalary e 
where mod(rownum,2)=1

--12
create table table2 as select * from EmployeeSalary where 1=2

--13
select * from EmployeeSalary1
intersect
select * from EmployeeSalary2

--14
select * from EmployeeSalary1
minus
select * from EmployeeSalary2

--15
select sysdate from dual;

--16
select * from EmployeeDetails where dateofJoining between '01/01/2016' and '31/12/2016'

--17
select * from EmployeeDetails where rownum<=7
select * from (select EmployeeDetails.*, dense_rank(order by empid desc) rn from  EmployeeDetails) where rn<=7
--18
select * from (select salary, dense_rank(order by salary desc) rn from EmployeeSalary) where rn<=7;

--19
select salary from EmployeeSalary e1 where 2=(select count(distinct(salary)) from EmployeeSalary e2 where e2.salary>e1.salary)
```




### 20 get the second highest salary among all Employees

### 21 retrieve alternate records from a table in Oracle

### 22 find Max salary and Department name from each department

### 23 find records in Table A that are not in Table B without using NOT IN operator

### 24 find employees that have same name and email

### 25 find Max salary from each department
### 26 Write SQL query to get the nth highest salary among all Employees
### 27 How can you find 10 employees with Odd number as Employee ID
### 28 Write a SQL Query to get the names of employees whose date of birth is between 01/01/1990 to 31/12/2000.
### 29 get the Quarter from date.
### find employees with duplicate email.
### Write a Query to find all Employee whose name contains the word "Rich", regardless of case. E.g. Rich, RICH, rich
```sql
select max(sal) from emp where sal != (select max(sal) from emp);

select * (select emp.*, rownum rn from emp) where mod(rn,2)=0

select max(sal)
```