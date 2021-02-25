# fetch first X rows only
Now we you can limit your SQL query result sets to a specified number of rows.
 ג€fetch first Nג€ turns into a hidden row_number() over() analytic function - so use ג€fetch first N rowsג€ instead of ג€where rownum <= Nג€ 
```sql
SELECT * FROM hr.employees ORDER BY salary FETCH FIRST 4 ROWS ONLY;
```
The following illustrates the syntax of the row limiting clause:
```
[ OFFSET offset ROWS]
 FETCH  NEXT [  row_count | percent PERCENT  ] ROWS  [ ONLY | WITH TIES ] 
```
[Here are some good examples](https://www.oracletutorial.com/oracle-basics/oracle-fetch/)

