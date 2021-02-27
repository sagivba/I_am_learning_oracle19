# static expressions
[livesql](https://livesql.oracle.com/apex/livesql/file/content_EDX8UZPUE3RO12C4ETRA88I8X.html)
[oracle doc](http://docs.oracle.com/database/122/LNPLS/plsql-language-fundamentals.htm#LNPLS300)
we can now use static expressions where previously only literal constants were allowed.

This will help us write code that adapts more easily and automatically to changes and is easier to maintain.

The length must be computable at compile time (that's what it means to be a static expression). 

But still, we can reference a length defined elsewhere (and in one place). 
WE can do this is a "regular" declaration and also in the definition of a subtype like so:

```sql
CREATE OR REPLACE PACKAGE pkg 
IS 
   c_max_length constant integer := 32767; 
   SUBTYPE maxvarchar2 IS VARCHAR2 (c_max_length); 
END;
/
DECLARE 
   l_big_string1 VARCHAR2 (pkg.c_max_length) := 'So big....'; 
   l_big_String2 pkg.maxvarchar2 := 'So big via packaged subtype....'; 
   l_half_big VARCHAR2 (pkg.c_max_length / 2) := 'So big....'; 
BEGIN    
   DBMS_OUTPUT.PUT_LINE (l_big_string1); 
   DBMS_OUTPUT.PUT_LINE (l_big_string2); 
END;
/
```



