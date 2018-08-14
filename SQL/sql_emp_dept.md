Part - I SQL Interview Questions

# Display employee number and names for employees who earn commission.
```sql
select empno, ename from emp where comm is not null and comm>0
```

# Display the names of employees who are working in the company for the past 5 years.
```sql
select ename from emp where systemdate-hiredate> 5*365
```

# Display the names of employees who are not working as managers.
```sql
select ename from emp where empno not in (select mgr from emp where mgr is not null)
```

# Display dept numbers and total number of employees within each group.
```sql
select deptno, count(*) from emp group by deptno
```

# Display the names of the employees who earn highest salary in their respective departments.
```sql
select ename from emp e where sal = (select max(sal) from emp where deptno=e.deptno)
```

# Display the names of employees who earn highest salaries in their respective job groups.
```sql
select * from emp e where sal = (select max(sal) from emp where e.job=job)
```

# Display the names of employees from department number 10 with salary greater than that of any employee working in other departments.
```sql
select ename,sal,deptno from emp e where deptno=10 and sal > any(select sal from emp where e.deptno!=deptno);
```

# Use appropriate function and extract 3 characters starting from 2 characters from the following string ?Oracle? i.e. the output should be ?rac?.
```sql
-- rac
select substr('oracle',2,3) from dual;
--17   2nd 'a' , start from index 1
select instr('computer maintenance corporation','a',1,2) from dual;
-- 'bllens'
select replace('Allens','A','b') from dual;
```

18. Display empno, ename, deptno from EMP table. Instead of display department numbers display the
related department name (use decode function).
```
select empno, ename, decode(deptno, 10, 'ACCOUNTING', 20, 'RESEARCH', 30, 'SALES', 'DEFAULT') department from emp
```

27. Display the jobs found in department number 10 and 20 eliminate duplicate jobs.
```sql
select job from emp where deptno=10 
intersect 
select job from emp where deptno=20;
```

3. Display employee name, his job and his manager. Display also employees who are without manager.
```sql
select e.ename, e.job, m.ename from emp e left outer join emp m
on e.mgr = m.empno;
```

4. Find out the top 5 earner of company.
```sql
select * from (select emp.*, dense_rank() over (order by sal desc)
rn from emp) where rn<=5;
```

10. Display those managers name whose salary is more than an average salary of his employees.
```sql
select ename, sal from emp m where empno in (select mgr from emp) 
and m.sal>(select avg(sal) from emp where mgr=m.empno);
```

11. Display employee name, Sal, comm and net pay for those employees whose net pay are greater than or equal to any other employee salary of the company?
```sql
select ename, sal, comm, sal+nvl(comm,0) netPay from emp where sal+nvl(comm.,0)>=any(select sal from emp);
```
12. Display those employees whose salary is less than his manager but more than salary of any other employees.
```sql
select * from emp e where sal<(select sal from emp where empno = e.mgr) and sal>any(select sal from emp where empno!=e.mgr);
```

16. Display those employee who are not working under president but they are working under any other manager?
```sql
select * from emp e where mgr in(select empno from emp where ename<>'KING');
```

17. Delete those department where no employee working?
delete from dept d where 0=(select count(*) from emp where deptno=d.deptno);
18. Delete those records from EMP table whose deptno not available in dept table?
delete from emp where deptno not in(select deptno from dept);
19. Display those earners whose salary is out of the grade available in Sal grade table?
select * from emp where sal<(select min(losal) from salgrade) or sal>(select max(hisal) from
salgrade);
20. Display employee name, Sal, comm. and whose net pay is greater than any other in the company?
Select ename, sal, comm from emp where sal+sal*15/100-sal*5/100 +sal*10/100 = (select
max(sal+sal*15/100-sal*5/100+sal*10/100) from emp);
21. Display name of those employees who are going to retire 31-dec-99. If the maximum job is period
is 18 years?
select * from emp where (to_date('31-dec-1999')-hiredate)/365>18;
22. Display those employees whose salary is ODD value?
select * from emp where mod(sal,2)=1;
23. Display those employees whose salary contains at least 4 digits?
select * from emp where length(sal)>=4;
24. Display those employees who joined in the company in the month of DEC?
select * from emp where upper(to_char(hiredate,'mon'))='DEC';
25. Display those employees whose name contains ?A??
select * from emp where instr(ename,'A',1,1)>0;
26. Display those employees whose deptno is available in salary?
select * from emp where instr(sal,deptno,1,1)>0;
27. Display those employees whose first 2 characters from hire date-last 2 characters of salary?
select substr(hiredate,0,2)||substr(sal,length(sal)-1,2) from emp;
select concat( substr(hiredate,0,2), substr(sal,length(sal)-1,2) ) from emp;
28. Display those employees whose 10% of salary is equal to the year of joining?
select * from emp where to_char(hiredate,'yy')=sal*10/100;
29. Display those employees who are working in sales or research?
8 of 13
select * from emp where deptno in(select deptno from dept where dname in('SALES','RESEARCH'));
30. Display the grade of Jones?
select grade from salgrade where losal<=(select(sal) from emp where ename='JONES') and
hisal>=(select(sal) from emp where ename='JONES');
31. Display those employees who joined the company before 15th of the month?
select empno,ename from emp where hiredate<(to_date('15-'||to_char(hiredate,'mon')||'-
'||to_char(hiredate,'yyyy')));
32. Delete those records where no of employee in a particular department is less than 3?
delete from emp where deptno in(select deptno from emp group by deptno having count(*)>3);
33. Delete those employees who joined the company 21 years back from today?
select * from emp where round((sysdate-hiredate)/365)>21; or
select * from emp where (to_char (sysdate, 'yyyy')-to_char (hiredate ,'yyyy') )>21;
34. Display the department name the no of characters of which is equal to no of employees in any other
department?
Select dname from dept where length(dname) in (select count(*) from emp group by deptno);
35. Display those employees who are working as manager?
select * from emp where empno in(select mgr from emp);
36. Count the no of employees who are working as manager (use set operation)?
select count(*) from emp where empno in(select mgr from emp);
37. Display the name of then dept those employees who joined the company on the same date?
select empno,ename,hiredate,deptno from emp e where hiredate in (select hiredate from emp where
empno<>e.empno);
38. Display those employees whose grade is equal to any number of Sal but not equal to first number of
Sal?
39. Display the manager who is having maximum number of employees working under him?
Select mgr from emp group by mgr having count(*)=(select max(count(mgr)) from emp group by
mgr);
40. List out employees name and salary increased by 15% and expressed as whole number of dollars?
select empno,ename,lpad(concat('$',round(sal*115/100)),7) salary from emp;
41. Produce the output of the EMP table ?EMPLOYEE_AND_JOB? for ename and job?
select * from EMPLOYEE_AND_JOB;
42. List all employees with hire date in the format ?June 4 1988??
select to_char(hiredate,'month dd yyyy') from emp;
43. Print a list of employees displaying ?Less Salary? if less than 1500 if exactly 1500 display as ?Exact
Salary? and if greater than 1500 display ?More Salary??
select empno,ename,'Less Salary '||sal from emp where sal<1500
union
select empno,ename,'More Salary '||sal from emp where sal>1500
union
select empno,ename,'Exact Salary '||sal from emp where sal=1500
44. Write query to calculate the length of employee has been with the company?
Select round(sysdate-hiredate) from emp;
45. Given a String of the format ?nn/nn? verify that the first and last 2 characters are numbers. And
that the middle characters is ?y? print the expressions ?Yes? if valid ?No? of not valid use the
following values to test your solution
46. Employees hire on 15th of any month are paid on the last Friday of that month. Those hired after
9 of 13
15th are paid the last Friday of the following month. print a list of employees their hire date and first
pay date sort those whose Sal contains first digits of their dept.
47. Display those mangers who are getting less than his employees Sal.
Select empno from emp e where sal<any(select sal from emp where mgr=e.empno);
48. Print the details of all the employees who are sub ordinate to Blake.
Select * from emp where mgr=(select empno from emp where ename='BLAKE');
Part – IV
1. Display those who working as manager using co related sub query.
Select * from emp where empno in(select mgr from emp);
2. Display those employees whose manger name is Jones and also with his manager name.
Select * from emp where mgr=(select empno from emp where ename='JONES') union select * from
emp where empno=(select mgr from emp where ename='JONES');
3. Define variable representing the expressions used to calculate on employee?s total annual
renumaration.
define emp_ann_sal=(sal+nvl(comm,0))*12;
4. Use the variable in a statement which finds all employees who can earn 30,000 a year or more.
select * from emp where &emp_ann_sal>30000;
5. Find out how many mangers are there with out listing them.
Select count (*) from EMP where empno in (select mgr from EMP);
6. Find out the avg sal and avg total remuneration for each job type remember salesman earn
commission.
select job,avg(sal+nvl(comm,0)),sum(sal+nvl(comm,0)) from emp group by job;
7. Check whether all employees number are indeed unique.
select count(empno),count(distinct(empno)) from emp having
count(empno)=(count(distinct(empno)));
8. List out the lowest paid employees working for each manager, exclude any groups where min sal is
less than 1000 sort the output by sal.
select e.ename,e.mgr,e.sal from emp e where sal in(select min(sal) from emp where mgr=e.mgr)
and e.sal>1000 order by sal;
9. list ename, job, annual sal, deptno, dname and grade who earn 30000 per year and who are not
clerks.
Select e.ename, e.job, (e.sal+nvl(e.comm,0))*12, e.deptno, d.dname, s.grade from emp e, salgrade
s , dept d where e.sal between s.losal and s.hisal and e.deptno=d.deptno and
(e.sal+nvl(comm,0))*12> 30000 and e.job <> 'CLERK';
10. find out the job that was failed in the first half of 1983 and the same job that was failed during the
same period on 1984.
11. find out the all employees who joined the company before their manager.
Select * from emp e where hiredate<(select hiredate from emp where empno=e.mgr);
12. list out the all employees by name and number along with their manager?s name and number also
display ?No Manager? who has no manager.
select e.empno,e.ename,m.empno Manager,m.ename ManagerName from emp e,emp m where
e.mgr=m.empno
union
select empno,ename,mgr,'No Manager' from emp where mgr is null;
13. find out the employees who earned the highest Sal in each job typed sort in descending Sal order.
select * from emp e where sal =(select max(sal) from emp where job=e.job);
14. find out the employees who earned the min Sal for their job in ascending order.
select * from emp e where sal =(select min(sal) from emp where job=e.job) order by sal;
15. find out the most recently hired employees in each dept order by hire date
10 of 13
select * from emp order by deptno,hiredate desc;
16. display ename, sal and deptno for each employee who earn a Sal greater than the avg of their
department order by deptno
select ename,sal,deptno from emp e where sal>(select avg(sal) from emp where deptno=e.deptno)
order by deptno;
17. display the department where there are no employees
select deptno,dname from dept where deptno not in(select distinct(deptno) from emp);
18. display the dept no with highest annual remuneration bill as compensation.
select deptno,sum(sal) from emp group by deptno having sum(sal) = (select max(sum(sal)) from
emp group by deptno);
19. In which year did most people join the company. Display the year and number of employees
select count(*),to_char(hiredate,'yyyy') from emp group by to_char(hiredate,'yyyy');
20. display avg sal figure for the dept
select deptno,avg(sal) from emp group by deptno;
21. Write a query of display against the row of the most recently hired employee. display ename hire
date and column max date showing.
select empno,hiredate from emp where hiredate=(select max (hiredate) from emp);
22. display employees who can earn more than lowest Sal in dept no 30
select * from emp where sal>(select min(sal) from emp where deptno=30);
23. find employees who can earn more than every employees in dept no 30
select * from emp where sal>(select max(sal) from emp where deptno=30);
select * from emp where sal>all(select sal from emp where deptno=30);
24. select dept name dept no and sum of Sal
break on deptno on dname;
select e.deptno,d.dname,sal from emp e, dept d where e.deptno=d.deptno order by e.deptno;
25. find out avg sal and avg total remainders for each job type
26. find all dept?s which have more than 3 employees
select deptno from emp group by deptno having count(*)>3;
27. If the pay day is next Friday after 15th and 30th of every month. What is the next pay day from
their hire date for employee in emp table
28. If an employee is taken by you today in your organization. And is a policy in your company to have
a review after 9 months the joined date (and of 1st of next month after 9 months) how many days
from today your employee has to wait for a review
29. Display employee name and his sal whose sal is greater than highest avg of dept no
30. Display the 10th record of EMP table. (without using rowid)
31. Display the half of the enames in upper case and remaining lower case
select concat ( upper ( substr ( ename, 0 , length (ename)/ 2) ),
lower (substr (ename, length(ename) / 2+1, length(ename) )) ) from emp;
32. display the 10th record of emp table without using group by and rowed
33. Delete the 10th record of emp table.
34. Create a copy of emp table.
Create table emp1 as select * from emp;
35. Select ename if ename exists more than once.
select distinct(ename) from emp e where ename in(select ename from emp where
e.empno<>empno);
11 of 13
36. display all enames in reverse order.
select ename from emp order by ename desc;
37. Display those employee whose joining of month and grade is equal.
select empno,ename from emp e, salgrade s where e.sal between s.losal and s.hisal and
to_char(hiredate,'mm')=grade;
38. Display those employee whose joining date is available in dept no
select * from emp where to_char(hiredate,'dd')=deptno;
39. Display those employees name as follows A ALLEN, B BLAKE
select substr(ename,1,1)||' '||ename from emp;
40. List out the employees ename, sal, PF from emp
Select ename,sal,sal*15/100 PF from emp;
41. Display RSPS from emp without using updating, inserting
42. Create table emp with only one column empno
Create table emp (empno number(5));
43. Add this column to emp table ename Varchar(20).
alter table emp add ename varchar2(20) not null;
44. OOPS! I forgot to give the primary key constraint. Add it now.
alter table emp add constraint emp_empno primary key (empno);
45. Now increase the length of ename column to 30 characters.
alter table emp modify ename varchar2(30);
46. Add salary column to emp table.
alter table emp add sal number(7,2);
47. I want to give a validation saying that sal cannot be greater 10,000(note give a name to this
column).
alter table emp add constraint emp_sal_check check (sal<10000);
48. For the time being I have decided that I will not impose this validation. My boss has agreed to pay
more than 10,000.
Alter table emp disable constraint emp_sal_check;
49. My boss has changed his mind. Now he doesn?t want to pay more than 10,000. So revoke that
salary constraint
Alter table emp enable constraint emp_sal_check;
50. Add column called as mgr to your emp table.
Alter table emp add mgr number(5);
51. Oh! This column should be related to empno. Give a command to add this constraint
Alter table emp add constraint emp_mgr foreign key(empno);
52. Add dept no column to your emp table
Alter table emp add deptno number(3);
53. This dept no column should be related to deptno column of dept table
Alter table emp1 add constraint emp1_deptno foreign key(deptno) references dept(deptno);
54. Create table called as new emp. Using single command create this table as well as to get data into
this table (use create table as)
create table newemp as select *from emp;
55. Create table called as newemp. This table should contain only empno,ename, dname
12 of 13
create table newemp as select empno,ename,dname from emp e , dept d where e.deptno=d.deptno;
56. Delete the rows of employees who are working in the company for more than 2 years.
Delete from emp where floor(sysdate-hiredate)>2*365;
57. Provide a commission to employees who are not earning any commission.
update emp set comm=300 where comm is null;
58. If any employee has commission his commission should be incremented by 10% of his salary.
update emp set comm=comm*10/100 where comm is not null;
59. Display employee name and department name for each employee.
select ename,dname from emp e, dept d where e.deptno=d.deptno;
60. Display employee number, name and location of the department in which he is working.
Select empno, ename, loc from emp e, dept d where e.deptno=d.deptno;
61. Display ename, dname even if there no employees working in a particular department(use outer
join).
Select ename, dname from emp e, dept d where e.deptno (+)= d.deptno;
62. Display employee name and his manager name.
Select e.ename, m.ename from emp e, emp m where e.mgr=m.empno;
63. Display the department name along with total salary in each department.
Select deptno, sum(sal) from emp group by deptno;
64. Display the department name and total number of employees in each department.
select deptno,count(*) from emp group by deptno;
65. Alter table emp1 add constraint emp1_deptno foreign key(deptno) references dept(deptno)
66. Delete from emp where job name is clerk
67. Insert into emp without giving any further commands move to another client system and log into
the same user give the following command
68. Are the above changes reflected in this user
69. Go to your fist system and give commit come back to the second system and give the following
command
70. Display the current date and time
select to_char(sysdate,'month mon dd yy yyyy hh:mi:ss') from dual;
71. List all the employees who have at least one person reporting to them.
SELECT DISTINCT(A.ENAME) FROM EMP A, EMP B WHERE A.EMPNO = B.MGR; or SELECT ENAME
FROM EMP WHERE EMPNO IN (SELECT MGR FROM EMP);
72. List the name of the employees with their immediate higher authority.
SELECT A.ENAME "EMPLOYEE", B.ENAME "REPORTS TO" FROM EMP A, EMP B WHERE
A.MGR=B.EMPNO;
73. Which department has the highest annual remuneration bill?
SELECT DEPTNO, LPAD(SUM(12*(SAL+NVL(COMM,0))),15) "COMPENSATION" FROM EMP GROUP BY
DEPTNO HAVING SUM( 12*(SAL+NVL(COMM,0)))=(SELECT MAX(SUM(12*(SAL+NVL(COMM,0))))
FROM EMP GROUP BY DEPTNO);
74. Write a query to display a ?*? against the row of the most recently hired employee.
SELECT ENAME, HIREDATE, LPAD('*',8) "RECENTLY HIRED" FROM EMP WHERE HIREDATE =
(SELECT MAX(HIREDATE) FROM EMP) UNION SELECT ENAME NAME, HIREDATE, LPAD(' ',15)
"RECENTLY HIRED" FROM EMP WHERE HIREDATE != (SELECT MAX(HIREDATE) FROM EMP);
75. Write a correlated sub-query to list out the employees who earn more than the average salary of
their department.
SELECT ENAME,SAL FROM EMP E WHERE SAL > (SELECT AVG(SAL) FROM EMP F WHERE E.DEPTNO
= F.DEPTNO);
13 of 13
76. Find the nth maximum salary.
SELECT ENAME, SAL FROM EMP A WHERE &N = (SELECT COUNT (DISTINCT(SAL)) FROM EMP B
WHERE A.SAL<=B.SAL);
77. Select the duplicate records (Records, which are inserted, that already exist) in the EMP table.
SELECT * FROM EMP A WHERE A.EMPNO IN (SELECT EMPNO FROM EMP GROUP BY EMPNO HAVING
COUNT(EMPNO)>1) AND A.ROWID!=MIN (ROWID));
78. Write a query to list the length of service of the employees (of the form n years and m months).
SELECT ENAME "EMPLOYEE",TO_CHAR(TRUNC(MONTHS_BETWEEN(SYSDATE,HIREDATE)/12))||'
YEARS '|| TO_CHAR(TRUNC(MOD(MONTHS_BETWEEN (SYSDATE, HIREDATE),12)))||' MONTHS '
"LENGTH OF SERVICE" FROM EMP;
79. what is the o/p of the below query?
select rownum rank,ename,sal from (select ename,sal from emp order by sal desc)
where rownum<=4
minus
select rownum,emp.ename,emp.sal from emp where 1=(select count(distinct b.sal) from emp b
where b.sal>=emp.sal)
80. To get nth sal ?
select a.* from emp a where &n>= (select count(distinct b.sal) from emp b where a.sal<=b.sal)
minus
(select a.* from emp a where 1= (select count(distinct b.sal) from emp b where a.sal<=b.sal))
80. To Select SAL and its occurences according to its order in assending
select sal,count(*) from emp group by sal order by sal
81. Selecting Nth Maximum Salary from emp
select ename,sal from emp a where &n = (select count(distinct(sal)) from emp b where
b.sal>=a.sal)
82. Selecting Nth Minimum Salary from emp
select ename,sal from emp a where &n = (select count(distinct(sal)) from emp b where b.sal <=
a.sal)
83. Selecting Salary and Rownum from Emp select sal,rownum from emp order by sal
85. Selecting Alternative Rows From A Table
select empno,ename,sal from(select empno,ename,sal , mod(rownum,2) a from emp ) b where
b.a=0
86. Insert values from one table to another table
Insert into empdup select * from emp Note: AS and VALUES keywords are not used in this statement
87. Deleting duplicate rows from a table
delete empdup a where rowid <>
(select max(rowid) from empdup b where a.empno = b.empno)
88. To Print A Number In Words
select to_char(to_date(1234,'JSP'),'JSP') from dual
89. To Find The No Of Columns In A table
select count(*) from user_tab_columns where table_name = 'EMP'
90. To Get The SALs greater than 1000 in each DEPT
select sal,count(sal),dname from emp a,dept b where a.deptno = b.deptno group by sal,dname
having sal>1000 order by dname
91. To print each Dept Having Sal Greater than 1000 including the sal,deptno,
their count
select dname,a.deptno,sal,count(*) from emp a,dept b where a.deptno=b.deptno having sal>1000 group
by a.deptno,dname,sal order by dname