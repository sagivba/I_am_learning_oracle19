# Optiomizing function
## UDF pragma 
* [mwidlake](https://mwidlake.wordpress.com/2015/11/04/pragma-udf-speeding-up-your-plsql-functions-called-from-sql/)

The ```UDF``` pragma tells the compiler that the PL/SQL unit is a user defined function that is used *primarily in SQL statements*, 
which might improve its performance by prebventing conact swiching between the sql engine and plsql engine.

As of Oracle Database 12c, there is also the possibility of adding a PL/SQL function to your SQL statement with the ```WITH``` clause. 
A non-trivial example is described on Databaseline, from which it follows that the WITH clause is marginally faster than the UDF pragma, 
but that the latter has the advantage that it is modular, whereas the former is the equivalent of hard coding your functions.

if and only if they are called from SQL statements but not PL/SQL code,
Oracle recommends that you first try to add ```PRAGMA UDF``` to your PL/SQL functions.
 
*If that does not provide a significant benefit, then try the ```WITH function block```.*

One thing to keep in mind: the performance of the UDF-ied function could actually degrade a bit when run natively in PL/SQL (outside of a SQL statement).

## PRAGMA UDF and WITH clause enhancements

[oracle-base](https://oracle-base.com/articles/12c/with-clause-enhancements-12cr1)

In a number of presentations prior to the official 12c release, 
speakers mentioned PRAGMA UDF (User Defined Function), 
which supposedly gives you the performance advantages of inline PL/SQL, 
while allowing you to define the PL/SQL object outside the SQL statement. 
The following code redefines the previous normal function to use this pragma.

```sql
WITH 
 function get_commision(insal in number) return number is 
 BEGIN
 RETURN insal*.10;
 END;
SELECT empno,name,sal,get_commision(sal) commision FROM t2;
/
```

or just
```sql
create or replace function get_commision(insal in number) return number is 
Pragma UDF;
begin
 return insal*.10;
end;
/

select empno,name,sal,get_commision(sal) commision from t2;
```


