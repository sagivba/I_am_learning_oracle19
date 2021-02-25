# mark elemets for depracation
[stevenfeuersteinonplsql blogspot](http://stevenfeuersteinonplsql.blogspot.com/2016/10/122-helps-you-manage-persistent-code.html)

We can now use the DEPRECATE pragma to document that a program unit (e.g., package) or subprogram (e.g., procedure in a package) is deprecated and should not be used. 
We can then take advantage of compile-time warnings to help identify all places that deprecated code is used.

## Turn on Compile-Time Warnings

```sql
ALTER SESSION SET plsql_warnings = 'enable:all'
/

CREATE OR REPLACE PACKAGE pkg 
AS 
   PRAGMA DEPRECATE(pkg); 
 
   PROCEDURE proc; 
   FUNCTION func RETURN NUMBER; 
END;
/
```

## Mark Entire Package as Deprecated

```sql
CREATE OR REPLACE PACKAGE pkg 
   AUTHID DEFINER 
AS 
   PRAGMA DEPRECATE(pkg); 
 
   PROCEDURE proc; 
   FUNCTION func RETURN NUMBER; 
END;

CREATE OR REPLACE PACKAGE BODY pkg
AS
   PROCEDURE proc
   IS
   BEGIN
      DBMS_OUTPUT.put_line ('old stuff');
   END;

   FUNCTION func
      RETURN NUMBER
   IS
   BEGIN
      RETURN 1;
   END;
END;
```


*Warning:* PACKAGE PKG
Line: 4 PLW-06019: entity PKG is deprecated

## Now let's change Warnings into Errors:

```sql
ALTER SESSION SET plsql_warnings='ERROR:(6019,6020,6021,6022)'
/

CREATE OR REPLACE PACKAGE pkg 
AS 
   PRAGMA DEPRECATE(pkg); 
 
   PROCEDURE proc; 
   FUNCTION func RETURN NUMBER; 
END;
/
```

*Errors:* PACKAGE PKG
Line: 4 PLS-06019: entity PKG is deprecated




