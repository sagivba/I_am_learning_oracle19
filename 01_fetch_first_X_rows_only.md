# fetch first X rows only
Now we you can limit your SQL query result sets to a specified number of rows.
 ```fetch first N``` turns into a hidden row_number() over() analytic function - so use ```fetch first N rowsג``` instead of ```where rownum <= N``` 
```sql
SELECT * FROM hr.employees ORDER BY salary 
FETCH FIRST 4 ROWS ONLY;
```
The following illustrates the syntax of the row limiting clause:
```
[ OFFSET offset ROWS]
 FETCH  NEXT [  row_count | percent PERCENT  ] ROWS  [ ONLY | WITH TIES ] 
```
If we want all the rows with the 10th hifghet salery:
```sql
SELECT * FROM hr.employees ORDER BY salary 
FETCH NEXT 10 ROWS WITH TIES;
```


[Here are some good examples](https://www.oracletutorial.com/oracle-basics/oracle-fetch/)

