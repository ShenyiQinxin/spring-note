#### Report each person info, regardless if there is an address for each of people
```sql
select p.firstname, p.lastname, a.city, a.state
from person p left outer join address a 
on p.personid = a.personid
```
####
```sql
```