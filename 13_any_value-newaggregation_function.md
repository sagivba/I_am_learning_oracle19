# The `ANY_VALUE` new aggregations function
this should be on 21c but also it seems to work on 19.12C
thenk to [db-oriented](http://db-oriented.com/2020/12/20/any_value/) for that great tip!

The `ANY_VALUE` function allows us to safely drop columns out of a GROUP BY clause to reduce any performance overhead.
the examples are example (from the scott schema)

Count of the number of employees of each deptpartment:
```sql
select d.deptno, d.dname,  count(e.empno) as employees_number
from   dept d left outer join  emp e on d.deptno = e.deptno
group by d.deptno, d.dname
order by 1;
```
or 
```sql
select d.deptno, min(d.dname),  count(e.empno) as employees_number
from   scott.dept d left outer join  scott.emp e on d.deptno = e.deptno
group by d.deptno
order by 1;
```

But we *know* that every deptpartment name  (dname)is the same for the same deptno
how can we do thet 

## The solution 
Oracle 21c (works on 19.12C) introduced the ANY_VALUE aggregate function to solve this problem. 
We use it in the same way we would use MIN or MAX, but it is optimized to reduce the overhead of the aggregate function.

```sql
select d.deptno,  any_value(d.dname) as dname,
       count(e.empno) as employee_count
from   dept d left outer join  emp e on d.deptno = e.deptno
group by d.deptno
order by 1;
```
