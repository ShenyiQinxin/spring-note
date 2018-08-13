
Part - I SQL Interview Questions

#5. Display employee number and total salary for each employee.
#6. Display employee name and annual salary for all employees.
#7. Display the names of all employees who are working in department number 10.
#8. Display the names of all employees working as clerks and drawing a salary more than 3000.
#9. Display employee number and names for employees who earn commission.
#10. Display names of employees who do not earn any commission.
#11. Display the names of employees who are working as clerk, salesman or analyst and drawing a
#12. Display the names of employees who are working in the company for the past 5 years.
#13. Display the list of employees who have joined the company before 30th June 90 or after 31st dec 90.
#14. Display current date. 
15. Display the list of users in your database (using log table). 
16. Display the names of all tables from the current user. 
17. Display the name of the current user. show user;
18. Display the names of employees working in department number 10 or 20 or 40 or employees
working as clerks, salesman or analyst.
19. Display the names of employees whose name starts with alphabet S.
20. Display employee names for employees whose name ends with alphabet.

21. Display the names of employees whose names have second alphabet A in their names.

22. Display the names of employees whose name is exactly five characters in length.


23. Display the names of employees who are not working as managers.

24. Display the names of employees who are not working as SALESMAN or CLERK or ANALYST.
25. Display all rows from EMP table. The system should wait after every screen full of information.
set pause on;
26. Display the total number of employees working in the company. 
27. Display the total salary being paid to all employees.

28. Display the maximum salary from emp table. 
29. Display the minimum salary from emp table. 
30. Display the average salary from emp table. 
31. Display the maximum salary being paid to CLERK.

32. Display the maximum salary being paid in dept no 20.

33. Display the min Sal being paid to any SALESMAN.

34. Display the average salary drawn by managers.

35. Display the total salary drawn by analyst working in dept no 40.

36. Display the names of employees in order of salary i.e. the name of the employee earning lowest

37. Display the names of employees in descending order of salary.

38. Display the details from emp table in order of emp name.

39. Display empno, ename, deptno, and sal. Sort the output first based on name and within name by
deptno and within deptno by Sal;

40. Display the name of the employee along with their annual salary (Sal * 12). The name of the
employee earning highest annual salary should appear first.

41. Display name, Sal, hra, pf, da, total Sal for each employee. The output should be in the order of
total Sal, hra 15% of Sal, da 10% of sal, pf 5% of sal total salary will be (sal*hra*da)-pf.

42. Display dept numbers and total number of employees within each group.

43. Display the various jobs and total number of employees with each job group.
3 of 13
select job, count(*) from emp group by job;
44. Display department numbers and total salary for each department.
select deptno, sum(sal) from emp group by deptno;
45. Display department numbers and maximum salary for each department.
select deptno, max(sal),min(sal) from emp group by deptno;
46. Display the various jobs and total salary for each job.
select job, sum(sal) from emp group by job;
47. Display each job along with minimum sal being paid in each job group.
select job, min(sal) from emp group by job;
48. Display the department numbers with more than three employees in each dept.
select deptno, count(*) from emp group by deptno having count(*)>3;
49. Display the various jobs along with total sal for each of the jobs where total sal is greater than
40000.
select job, sum(sal) from emp group by job having sum(sal)>40000;
50. Display the various jobs along with total number of employees in each job. The output should
contain only those jobs with more than three employees.
select job, count(*) from emp group by job having count(*)>3;
51. Display the name of emp who earns highest sal.
select ename from emp where sal=(select max(sal) from emp);
52. Display the employee number and name of employee working as CLERK and earning highest salary
among CLERKS.
select empno, ename from emp where job='CLERK' and sal=(select max(sal) from emp where
job='CLERK');
53. Display the names of the salesman who earns a salary more than the highest salary of any clerk.
select ename from emp where job=?SALESMAN? and sal >
(select max(sal) from emp where job='CLERK');
54. Display the names of clerks who earn salary more than that of James of that of sal lesser than that
of Scott.
select ename from emp where job='CLERK' and sal<(select sal from emp where ename='SCOTT')
and sal>(select sal from emp where ename='JAMES');
55. Display the names of employees who earn a Sal more than that of James or that of salary greater
than that of Scott.
select ename from emp where sal <
(select sal from emp where ename='SCOTT') and sal >
(select sal from emp where ename='JAMES');
Part – II SQL Interview Questions
1. Display the names of the employees who earn highest salary in their respective departments.
select * from emp e where sal = (select max(sal) from emp where deptno=e.deptno)
2. Display the names of employees who earn highest salaries in their respective job groups.
select * from emp e where sal in (select max(sal) from emp group by job having e.job=job)
3. Display the employee names who are working in accountings dept.
select ename from emp where deptno = (select deptno from dept where dname=?ACCOUNTING?);
(or)
select ename from emp where deptno in (select deptno from dept where dname=?ACCOUNTING?);
4. Display the employee names who are working in Chicago.
select ename from emp where deptno = (select deptno from dept where loc=?CHICAGO?);
5. Display the job groups having total salary greater then the maximum salary for managers.
select job, sum(sal) from emp group by job having sum(sal) >
(select max(sal) from emp where job='MANAGER');
4 of 13
6. Display the names of employees from department number 10 with salary greater than that of any
employee working in other departments.
select ename,sal,deptno from emp e where deptno=10 and sal > any(select sal from emp where
e.deptno!=deptno);
7. Display the names of employee from department number 10 with salary greater then that of all
employee working in other departments.
select ename, sal, deptno from emp e where deptno=10 and sal > any(select sal from emp where
e.deptno != deptno);
8. Display the names of employees in Upper case. select upper(ename) from emp;
9. Display the names of employees in lower case. select lower(ename) from emp;
10. Display the name of employees in proper case. select initcap(ename) from emp;
11. Find out the length of your name using appropriate function. select length(?India?) from dual;
12. Display the length of all employees? names. select sum(length(ename)) from emp;
13. Display the name of the employee concatenate with EMP no.
select ename||empno from emp; (or) select concat(ename,empno) from emp;
14. Use appropriate function and extract 3 characters starting from 2 characters from the following
string ?Oracle? i.e. the output should be ?rac?.
select substr(?oracle?,2,3) from dual;
15. Find the first occurrence of character a from the following string ?computer maintenance
corporation?.
select instr(?computer maintenance corporation?,?a?,1,1) from dual;
16. Replace every occurrence of alphabet A with B in the string Allen?s (user translate function).
select replace('Allens','A','b') from dual;
17. Display the information from EMP table. Wherever job ?manager? is found it should be displayed as
boss(replace function).
select empno, ename, replace(job, 'MANAGER', 'Boss') JOB from emp;
18. Display empno, ename, deptno from EMP table. Instead of display department numbers display the
related department name (use decode function).
select e.empno, e.ename, d.dname from emp e,dept d where e.deptno = d.deptno;
19. Display your age in days.
select round(sysdate-to_date('15-aug-1947')) from dual;
20. Display your age in months.
select floor(months_between(sysdate,'15-aug-1947')) "age in months" from dual;
21. Display current date as 15th august Friday nineteen forty seven.
select to_char(sysdate,'ddth month day year') from dual;
22. Display the following output for each row from EMP table as ?scott has joined the company on
Wednesday 13th august nineteen ninety?.
select ename||' has joined the company on '||to_char(hiredate,'day ddth month year') from emp;
23. Find the date of nearest Saturday after current day.
select next_day(sysdate, 'SATURDAY') from dual;
24. Display current time.
select to_char(sysdate,'hh:mi:ss') Time from dual;
25. Display the date three months before the current date.
select add_months(sysdate,-3) from dual;
5 of 13
26. Display the common jobs from department number 10 and 20.
select job from emp where deptno=10 and job in(select job from emp where deptno=20);
(or)
select job from emp where deptno=10 intersect select job from emp where deptno=20;
27. Display the jobs found in department number 10 and 20 eliminate duplicate jobs.
select distinct(job) from emp where deptno=10 and job in(select job from emp where deptno=20);
(or)
select job from emp where deptno=10 intersect select job from emp where deptno=20;
28. Display the jobs which are unique to dept no 10.
select job from emp where deptno=10 minus select job from emp where deptno!=10;
(or)
select job from emp where deptno = 10 and job not in (select job from emp where deptno<>10);
29. Display the details of those who do not have any person working under them.
select empno from emp where empno not in (select mgr from emp where mgr is not null);
30. Display the details of employees who are in sales dept and grade is 3.
select * from emp where sal>=(select losal from salgrade where grade=3) and sal<=(select hisal
from salgrade where grade=3) and deptno=(select deptno from dept where dname='SALES');
31. Display those who are not managers and who are managers any one.
select * from emp where empno in(select mgr from emp) union
select * from emp where empno not in(select mgr from emp where mgr is not null);
32. Display those employees whose name contains not less than 4 chars.
Select * from emp where length(ename)>4;
33. Display those departments whose name start with ?S? while location name end with ?O?.
select * from dept where dname like 'S%' and loc like '%O';
34. Display those employees whose manager name is JONES.
select * from emp where mgr=(select empno from emp where ename='JONES');
35. Display those employees whose salary is more than 3000 after giving 20% increment.
select * from emp where sal*120/100 > 3000; (or)
select * from emp where sal+sal*20/100 > 3000;
36. Display all employees with there dept name.
select ename, dname from emp e, dept d where e.deptno = d.deptno;
37. Display ename who are working in sales dept.
select empno, ename from emp where
deptno=(select deptno from dept where dname='SALES');
38. Display employee name, deptname, salary and comm. for those Sal in between 2000 to 5000 while
location is Chicago.
select empno,ename,deptno from emp where deptno=(select deptno from dept where
loc='CHICAGO') and sal between 2000 and 5000;
39. Display those employees whose salary greater than his manager salary.
select * from emp e where sal>(select sal from emp where empno=e.mgr);
40. Display those employees who are working in the same dept where his manager is working.
select * from emp e where deptno = (select deptno from emp where empno=e.mgr);
41. Display those employees who are not working under any manger.
select * from emp where mgr is null or empno=mgr;
42. Display grade and employees name for the dept no 10 or 30 but grade is not 4, while joined the
company before 31-dec-82.
select empno,ename,sal,deptno,hiredate,grade from emp e,salgrade s where e.sal>=s.losal and
e.sal<=s.hisal and deptno in(10,30) and grade<>4 and hiredate<'01-dec-1981';
6 of 13
43. Update the salary of each employee by 10% increments that are not eligible for commission.
update emp set sal=sal+(sal*10/100) where comm is null;
44. Delete those employees who joined the company before 31-dec-82 while there dept location is
?NEW YORK? or ?CHICAGO?.
delete from emp where hiredate<'31-dec-1982' and deptno in
(select deptno from dept where loc in('NEW YORK','CHICAGO'));
45. Display employee name, job, deptname, location for all who are working as managers.
select ename,job,dname,loc from emp e, dept d where e.deptno=d.deptno and empno in (select mgr
from emp);
46. Display those employees whose manager names is Jones, and also display there manager name.
select e.empno, e.ename, m.ename MANAGER from emp e, emp m
where e.mgr=m.empno and m.ename='JONES';
47. Display name and salary of ford if his Sal is equal to high Sal of his grade.
select ename,sal from emp e where ename='FORD' and sal=(select hisal from salgrade where
grade=(select grade from salgrade where e.sal>=losal and e.sal<=hisal));
Part – III SQL Interview Questions
1. Display employee name, his job, his dept name, his manager name, his grade and make out of an
under department wise.
break on deptno;
select d.deptno, e.ename, e.job, d.dname, m.ename, s.grade from
emp e, emp m, dept d, salgrade s where e.deptno=d.deptno and e.sal between s.losal and s.hisal
and e.mgr=m.empno order by e.deptno;
2. List out all the employees name, job, and salary grade and department name for every one in the
company except ?CLERK?. Sort on salary display the highest salary.
select empno, ename, sal, dname, grade from emp e, dept d, salgrade s where e.deptno=d.deptno
and e.sal between s.losal and s.hisal and e.job<>'CLERK' order by sal;
3. Display employee name, his job and his manager. Display also employees who are without
manager.
select e.ename, e.job, m.ename Manager from emp e,emp m where e.mgr=m.empno union select
ename,job,'no manager' from emp where mgr is null;
4. Find out the top 5 earner of company.
select * from emp e where 5>(select count(*) from emp where sal>e.sal) order by sal desc;
5. Display the name of those employees who are getting highest salary.
select empno,ename,sal from emp where sal=(select max(sal) from emp);
6. Display those employees whose salary is equal to average of maximum and minimum.
select * from emp where sal=(select (max(sal)+min(sal))/2 from emp);
7. Display count of employees in each department where count greater than 3.
select deptno, count(*) from emp group by deptno having count(*)>3;
8. Display dname where at least 3 are working and display only dname.
select dname from dept where deptno in
(select deptno from emp group by deptno having count(*)>3);
9. Display name of those managers name whose salary is more than average salary of company.
select ename, sal from emp where empno in(select mgr from emp) and sal > (select avg(sal) from
emp);
10. Display those managers name whose salary is more than an average salary of his employees.
select ename, sal from emp e where empno in(select mgr from emp) and e.sal>(select avg(sal)
from emp where mgr=e.empno);
11. Display employee name, Sal, comm and net pay for those employees whose net pay are greater
7 of 13
than or equal to any other employee salary of the company?
select ename, sal, comm, sal+nvl(comm,0) netPay from emp where sal+nvl(comm.,0)>=any(select
sal from emp);
12. Display those employees whose salary is less than his manager but more than salary of any other
managers.
select * from emp e where sal<(select sal from emp where empno = e.mgr) and sal>any(select sal
from emp where empno!=e.mgr);
13. Display all employees names with total Sal of company with employee name.
Select ename,
14. Find out the last 5(least) earner of the company?
select * from emp e where 5>(select count(*) from emp where sal<e.sal) order by sal;
15. Find out the number of employees whose salary is greater than there manager salary?
select count(*) from emp e where sal>(select sal from emp where empno=e.mgr);
16. Display those manager who are not working under president but they are working under any other
manager?
select * from emp e where mgr in(select empno from emp where ename<>'KING');
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