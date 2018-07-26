## Tables
### Dual table
1 row  1 column table
- for calculation;
- select psudo columns;
- sequence generator;  
```sql
//calculation
select 2+3 from dual;
//sysdate, user
select sysdate from dual;
//sequence generator
select rownum from dual connect by rownum<=10;
//insert more rows in dual: need to be an admin
insert into dual values ('Y')
```
### The easiest way to create a table/based on another table
```sql
create table emp0
as
select * from emp;
```
#### Can we drop all the columns from a table?
>drop all columns from a table is impossible
>we will get an error ORA-12983

### pseudo columns
>they behave like table columns, but not stroed in the tables
>we can SELECT from pseudo columns, but cannot INSERT/UPDATE/DELETE
- SYSDATE
- ROWID
- ROWNUM
- OBJECT_ID
- OBJECT_VALUE
- CURRVAL
- NEXTVAL
etc.

### count(*)/count(1)
>they are the same

## Filters
### IN and =
**IN** takes 999 values
**=** takes 1 value
```sql
select id, dept, sal
from emp
where id=1;

select id, dept, sal
from emp
where id in (100, 1, 122);
```
## EXIST and IN
>the inner query of IN is executed **1st and only once** , the list of values are used for outer query to evaluate
```sql
select id, dept_id, sal
from emp
where dept IN (select dept_id from dept);
```
>the inner query of EXISTS is executed for each row of outer query to check with the value of it. If the outer query executes N times, the inner query will execute N times as well.
```sql
select id, dept_id, sal
from emp
where EXISTS (select dept_id from dept);
```
## HAVING and WHERE
- **HAVING** is used to filter out data after the aggregation (GROUP BY).
- **WHERE** is used to filter out data before the aggregation (GROUP BY) 
```sql
select dept_id, sum(sal)
from emp
where dept_id in (100, 200, 90)
group by dept_id
having sum(sal) >10000;
```

## Delete and Truncate
- DELETE is to remove some rows from a table using WHERE , if no WHERE then all rows are removed. need COMMIT/ROLLBACK the transaction. there is a DELETE trigger. There is a integrity constraint if the pk is a fk in another table, which does not support delete cascade.
- TRUNCATE is to remove all rows from a table. cannot rollback. no index/trigger/dependencies are affected. faster then delete.

|DELETE|TRUNCATE  |
|--|--|
| selectively removes rows using WHERE condition|removes all rows  |
|if no WHERE, all rows removed|no WHERE|
|COMMIT/ROLLBACK|no rollback|
|DELETE triggers are fired|no triggers/indexes/dependencies need to be taken care of|
|slower|quicker, cus no dependencies|
|DML|DDL|


### Delete record from >=2 tables
> use inner join to choose rows matching both tables
```sql
delete from(
select 
froemp inner join dept
on emp.deptno = dept.deptno);
```
### Delete duplicate records
> select the minium rowid among duplicated rows , then delete the rows larger than the minimum rowid
```sql
delete from emp e1
where e1.rowid>(
select min(e2.rowid)
from emp e2
where e1.empno=e2.empno)
```

### JOINs
| inner join | left/right outer join|full outer join|cross join|natural join
|--|--|--|--|--|
| return rows that match the condition | The LEFT JOIN returns all records from the left table, and only the matched records from the right table. It preserves the unmatched rows from the left table, joining them with NULL rows in the shape of the right table. ||cartesian product|join condition is based on columns from both tables with the same names and types|
|**USING** for same name but different types|||when join condition is missing, it performs cartesion join|
|**ON** *(t1.c=t2.c)* or *(t1.c BETWEEN t2.low AND t2.high)*|||each row in emp joins with each row in dept| 
```sql
//DEPTNO	DNAME	LOC
select * from dept;
//
select * from emp;
-- left outer join
/*

*/
//DEPTNO DNAME	LOC	EMPNO ENAME	JOB	MGR	HIREDATE SAL COMM DEPTNO
//40	OPERATIONS	BOSTON	-	-	-	-	-	-	-	-
select * from dept d left outer join emp e on e.deptno=d.deptno;
//right outer join
//DEPTNO DNAME LOC	EMPNO ENAME JOB	MGR	HIREDATE SAL COMM DEPTNO
//-	-	-	-	-	-	-	-	40	OPERATIONS	BOSTON
select * from dept d right outer join emp e on e.deptno=d.deptno;
//return the rows from emp and dept where the condition matches
//EMPNO	ENAME	JOB	MGR	HIREDATE	SAL	COMM	DEPTNO	DEPTNO	DNAME	LOC
select * from emp e inner join dept d on e.deptno=d.deptno
//inner join on join on, the 2nd join condition could reference columns from all 3 tables
select * from emp e inner join dept d on (e.deptno=d.deptno) join t3 on (e.deptno=t3.deptno);
select deptno, emp.empno from emp inner join dept using (deptno, loc);
//inner join using is the modified version of natural join: same name different types, using could be with multi columns
select deptno, emp.ename, dept.loc from emp natural join dept;
//deptno is the join column, no prefix for deptno
//if deptno of emp and dept are the same name and type, 
//then 2 tables are put together as:
//join column, first table, second table
//DEPTNO EMPNO ENAME JOB MGR HIREDATE SAL COMM	DNAME	LOC
select * from emp cross join dept;
//14 * 4
select * from emp, dept;
```

### self join examples
- employee info and manager info are in the same table
>getting details about employee and manager
>get the employees who have same job as employee "BLAKE"
```sql
select e1.ename employee, e2,ename manager
from emp e1 inner join emp e0
on e1.mgr = e0.empno
//
select e2.ename
from emp e1 inner join emp e2
on e1.job = e2.job
where e1.ename='BLAKE'
```
## Data types
### In Oracle
- character types
CHAR `col1 char(10)  _____entry  length(col1) is 10`
VARCHAR `col1 varchar2(25)  entry  length(col) is 5`
VARCHAR2
NCHAR and NVARCHAR2 
>NCHAR is for fixed length unicode data for different language, 2n bytes
>CHAR is for character data, n bytes
LONG
> no boolean type !
- numeric types
NUMBER
BINARY_FLOAT
BINARY_DOUBLE
- date
**DATE** stores month ~ second
**TIMESTEMP** stores ~ milliseconds
```sql
trunc(sysdate)+0.123456=2018-03-30 1234.56  -->6 digits
```
**TIMESTEMP WITH TIME ZONE**
```sql
select cast(sysdate as timestamp with time zone) from dual;
```
etc.
- large objects
CLOB 
NCLOB
BFILE
- XML 
XMLTYPE
## Functions
### instr() & substr()
```sql
//Nanette
//the index of the 1st 'e' in ename -->4
select ename, instr(ename, 'e'),
//the index of the 1st 'K' in enam, starting from index 2-->4
instr(ename, 'e', 2),
//the index of the 2nd 'K' in enam, starting from index 1 --> 7
instr(ename, 'e', 1, 2),
//the index of 'et' --> 4
instr(ename, 'et'),
from emp
where deptno=10;
//start index, length
substr(ename, 1, 3) -->Nan
substr(ename,2) -->anette
substr(ename, -3) -->tte 
```
### concat(1st, 2nd);  ||
```sql
concat('Lulu','Wang') --> LuluWang
ename || ' ' || job || ' ' || sal -->lulu se 60k
```
### conversion funcs: to_char(), to_number(), to_date()
```sql
'100' convert char to number or number to char
'21-sep-05' convert char to date or date to char

	to_number()	to_date()
number <---- char ----> date
	   ---->      <----
	 to_char()       to_char()

to_char(sysdate, 'dd-mm-yyyy hh:mi:ss AM'); --> 12-03-1983 12:12:12 PM
to_char(sysdate, 'yyyy'); -->1989
to_char(1598, '9999'); --->1598
to_char(1598, '$9G999D9);  --->$1,598.0
to_date('10-11-2015', 'dd-mm-yyyy'); -->10-NOV-15
to_number('$1,000', '$9G999'); --> 1000
```
### lpad() & rpad()
```sql
lpad(sal, 10, '#') -->#####24000
rpad(sal, 10, '#') --> 24000#####
```
### single row function & multi row function
>Single-row functions
```sql
upper('Ellen'); ->ELLEN
lower('Ellen'); -->ellen
initcap('Ellen'); --> Ellen
round(10.5); --> 11
round(10.3); -->10
round(10.008,2); --> 10.01
round(90.008, -2); --> 100
round(16-JAN-01, 'MONTH'); --> 01-FEB-01
trunc(16-JAN-01, 'MONTH'); --> 01-JAN-01
trunc(10.8); --> 10
trunc(10.008, 2); --> 10
trunc(10.008,-2); --> 0
//the remainder of division
//oftern use to divide 2, to see the number is odd or even
mode(15,2); -->1 odd
mode(16,2);-->0 even
sysdate -->05-jul-17
sysdate + 3 --> adding 3 days
sysdate + 5/24 --> adding 5 hours to the sysdate 05-JUL-17
//how many weeks the employees 'Adam' work from hire day till now
select sysdate-hire_date "no of days", (sysdate-hire_date)/7 "no of weeks"
from emp where ename='Adam';
months_between(sysdate, hire_date);
add_months(hire_date, 4);
add_month(hire_date,-2);
next_day(sysdate, 'FRIDAY'); //the next friday since sysdate
next_day(sysdate, 1);
last_day(sysdate); //the last day of this month
```
>multi-row functions
>ignore null value, except `count(*)`
```sql
sum(sal)
count(sal)
count(distinct sal)
count(*) -->count all the rows in the table
//NULL value is replaced by 0
count(nvl(sal,0));
max(sal)
min(sal)
avg(sal)
//CLARK, KING, MILLER
select listagg(ename, ', ') within group (order by ename) "emp_list"
from emp where deptno=10;

```
### Decode & Case
```sql
//case
select ename, job, sal, 
case when ename='KING' then sal*2 
	when job='CLERK' then sal+1000 
	else sal 
	end "revised_sal" 
from emp;
//decode
select ename, job, sal, 
decode (job, 'SALESMAN', sal*2, 
			 'CLERK',sal+1000, 
	    sal) "revised_sal" 
from emp;
``` 
### NULL
>NULL `+-*/n` is still NULL
> all data types can have NULL
```sql
//if comm = null, then "data not found"
select * from emp where comm IS null;
select * from emp where comm not like '%K';
select * from emp where comm not in (100,122,133);
select * from emp where comm not between 12 and 13;
select * from emp where comm != 50;
```

### NVL, NVL2, NULL IF
```sql
//data type should be the same
nvl(sal, 0); //null -- 0
nvl(job, 'no job yet'); //null -- 'no job yet'
nvl(hire_date, '01-jan-01'); 
nul2(comm, not_null_return, null_return);
nullif(first_name, last_name); //if they are equal, return NULL; if they are not equal, return first_name
//first none null value
coalesce(null, 2400, 2345); //--sal
//equals nested nvl();
nvl(nvl(null, 2400), sal)
```
### modify NULL column to NOT NULL column
if the column has null value, cannot modify the column to not null column.
```sql
alter table emp modify (ename not null);
```
## Integrity constraints
> to prevent entry of invalid data
> 
|user_constraints||
|--|--|
|NOT NULL||
|primary key|a column or set of columns that uniquely identifies a row in a table. values must be unique, not null. index is created|
|foreign key(referential integrity)|one or a combination of columns refers to a PK of parent table. values of FK must match the values in parent table or null. it is logical not physical pointers.|
|check|sysdate, currval, nextval, rowid,etc can not be referenced with check, cus psydocolumns are dynamic in nature. could use a trigger with a psydocolumn to check a column values|
|unique|a column or set of columns needs to have unique values.allow NULL values. multi unique constraints.|
```sql
create table emp_col_constraint(
eid number,
ename varchar(100),
sal number not null, 
gender char(1),
deptno number,
constraint ename_pk primary key (eid, ename),
constraint ename_u unique (sal, deptno),
constraint gender_c check (gender in ('M','F')),
constraint deptno_foreign_key foreign key (deptno) references dept (deptno) on delete cascade //(set null) 
);

select * from user_constraints where table_name='emp_col_constraint';

emp(child table, dependent) deptno references-> dept(deptno)
dept(parent table, the referenced)
//delete record from dept would be success
1. fk has on delete cascade
2. records on child table deleted first, then parent table
3. disable fk constraint
```
>disable a constraint
```sql
alter table emp
disable constraint constraint_name;
```
### before trigger- constraint- after trigger
### circular reference
2 tables with FK referencing each other, insertion of data into table becomes impossible.
### drop table with constraints
```sql
drop table emp cascade constraints;
```
### sequence
database object that auto generates continuous unique numbers. used when we need to populate a column with unique values.

