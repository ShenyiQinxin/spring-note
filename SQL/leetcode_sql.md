#### Report each person info, regardless if there is an address for each of people
```sql
select p.firstname, p.lastname, a.city, a.state
from person p left outer join address a 
on p.personid = a.personid
```

#### Output big countries' name, population and area from **world** table. The area should be bigger than 3 million square km or a the population should be more than 25 million.
```sql
select name, population, area
from world
where area>3000000 or population>25000000
```

#### Get the second highest salary from the Employee table. If there is no second highest salary, then the query should return `null`.
```sql
select max(salary) "SecondHighestSalary "
from Employee
where salary != (select max(salary) from Employee)
```

#### Given a table salary(id, name, sex, salary), that has m=male and f=female values in `sex` column. Swap all f and m values (i.e., change all f values to m and vice versa).
```sql
update salary
	set 
	sex = case sex
		  when 'm' then 'f' else 'm'
		  end;
```

#### Given a Weather table, write a SQL query to find all dates' Ids with higher temperature compared to its previous (yesterday's) dates. [Id(INT) | RecordDate(DATE) | Temperature(INT)]
```sql
select id
from Weather today
inner join Weather yesterday
on (today.temperature>yesterday.temperature)
where today.recorddate=yesterday.recorddate+1
```

#### The Employee table holds all employees including their managers. Finds out employees who earn more than their managers. Employee (Id | Name  | Salary | ManagerId)
```sql
select e.name 
from employees e inner join employees m
on e.managerid = m.id
where e.salary>m.salary
```

#### find all duplicate emails in a table named Person ( Id | Email)
```sql
select Email
from (select Email, count(Email) from person group by email) email_count_table
where email_count_table>1

```

#### Delete all duplicate email entries in a table named `Person`, keeping only unique emails based on its smallest Id. ( Id | Email)
```sql
delete larger
from person larger inner join person smaller
on larger.email=smaller.email
where larger.id>smaller.id
```

#### Suppose that a website contains two tables, the Customers table (Id | Name) and the Orders table( Id | CustomerId ). Write a SQL query to find all customers who never order anything.
```sql
select c.name Customers
from customers c left outer join Orders o
on c.id = o.customerid;
where o.id is null
```

#### Output ( id    | movie     |  description |  rating) movies with an odd numbered ID and a description that is not 'boring'. 
Order the result by rating.
cinema(id    | movie     |  description |  rating)
```sql
select *
from cinema
where mod(id,2)=1 and description != "boring"
order by rating desc
```

#### Courses (student | class), list out all classes which have more than or equal to 5 students.
```sql
select class
from Courses
group by class
having count(distinct student)>=5
```

####  Table point(x). find the shortest distance between two points in these points.
```sql
select min(abs(p2.x-p1.x)) shortest
from point p1 cross join point p2
where p1.x!=p2.x
```

#### friend_request(sender_id | send_to_id |request_date)
request_accepted(requester_id | accepter_id |accept_date)
Write a query to find the overall acceptance rate of requests rounded to 2 decimals, which is the number of acceptance divide the number of requests.
(accept_rate | 0.8)

```sql
select round(ifnull(
(select count(distinct requester_id, accepter_id) from request_accepted)/
(select count(distinct sender_id, send_to_id) from friend_request)
,0), 2) accept_rate 
from dual
-- follow up: return the accept rate but for every month
select if(request_table.request =0, 0.00, round((accept_table.accept/request_table.request),2)) accept_rate, accept_table.month
from (select count(distinct requester_id, accepter_id) accept, month(accept_date)  month from request_accepted) accept_table,
	(select count(distinct sender_id, send_to_id) request, month(request_date) month from friend_request) request_table
	
where accept_table.month = request_table.month
group by accept_table.month

```

#### Query the customer_number from the orders table for the customer who has placed the largest number of orders. It is guaranteed that exactly one customer will have placed more orders than any other customer.
orders (order_number | customer_number | order_date | required_date | shipped_date | status | comment)


```sql
select customer_number
from orders 
group by customer_number
order by count(order_number) desc
limit 1
```

#### writing a query to judge whether these three sides can form a triangle, assuming table triangle holds the length of the three sides x, y and z. Table (x, y, z) 
```sql
select x, y, z, 
case when x+y>z and x+z>y and y+z>x then 'Yes' else 'No'
end 'triangle'
from triangle

```

#### return the list of customers NOT referred by the person with id '2'. customer (id, name, referee_id)
```sql
select name
from customer
where referee_id != 2 or referee_id is null
```

#### Given three tables: salesperson, company, orders.
Output all the names in the table salesperson, who didn’t have sales to company 'RED'.
```sql
select s.name
from salesperson where sales_id not in 
(select o.sales_id from company c inner join orders o on c.com_id = o.com_id
where c.name='RED') 
```

#### Select all employee's name and bonus(some employee bonus is null) whose bonus is < 1000.
```sql
select e.name, b.bonus
from employee e left outer join bonus b on e.empid=bonus.empid
where b.bonus<1000 or b.bonus is null
```

#### Table `number` contains many numbers in column num including duplicated ones. write a SQL query to find the biggest number, which only appears once.
```sql
select max(t.num) "num"
from (
select num
from number
group by num
having count(num)=1) t
```
#### query all the consecutive available seats order by the seat_id using the following `cinema` (seat_id | free 0/1)
```sql
select distinct seat_id
from cinema a inner join cinema b
on abs(a.seat_id-b.seat_id)=1 and a.free=1 and b.free=1
order by seat_id

```

#### Employee ( Id | Name  | Salary | DepartmentId) Department (Id | Name )
 find employees who earn the top three salaries in each of the department. ( Department | Employee | Salary)

 ```sql
select d.name, e.name, e.salary
from 
(select * from (select employee.*, dense_rank() over (partition by DepartmentId order by salary) rn from employee)where rn<=3) e inner join department d on e.departmentid = d.id
```

#### Get the nth highest salary from the Employee(Id | Salary )table.
```sql
select salary from (
select salary, rownum rn from 
(select distinct salary from employee order by salary desc) 
where rn=4)
```

#### table Logs (Id | Num), given the above Logs table, 1 is the only number that appears consecutively for at least three times.
```sql
select distinct l1.num
from Logs l1, Logs l2, Logs l3
where l1.id=l2.id-1
and l2.id=l3.id-1
and l1.num=l2.num
and l2.num=l3.num
```
#### implement a dense_rank() on scores table(id, score)
```sql
select score as "Score", rn as "Rank"
from (select scores.*, dense_rank() over (order by score desc) rn from scores)
```

#### Employee ( Id | Name  | Salary | DepartmentId)
department (Id | Name)
find employees who earn the top three salaries in each of the department

```sql
select d.name, e.name, e.salary
from (select * from (select employee.*, dense_rank() over (partition by DepartmentId order by salary desc) rn from employee) where rn<=3) e inner join department d on e.departmentid = d.id
```

####
```sql
```