# UTL_CALL_STACK pacakge
```UTL_CALL_STACK```: Fine-grained execution call stack package
Let's explored the different ways you can obtain the error message / stack in PL/SQL:

## SQLERRM 
The original, traditional and not currently recommended function to get the current error message. 
It is not recommended because the next two options avoid a problem which you are unlikely to run into: 
The error stack will be truncated at 512 bytes, and you might lose some error information.

## DBMS_UTILITY
Returns the error message / stack, and will not truncate your string like SQLERRM will.
* `DBMS_UTILITY.FORMAT_CALL_STACK` - How did I get here
* `DBMS_UTILITY.FORMAT_ERROR_STACK` - what is error message / stack - this one will not truncate your string like SQLERRM will.
* `MS_UTILITY.FORMAT_ERROR_BACKTRACE` - On what line was the error raised

## UTL_CALL_STACK API 

[livesql](https://livesql.oracle.com/apex/livesql/file/content_CTG44K8HGN960Z0TRIEUVXEQI.html)

Added in Oracle Database 12c, the UTL_CALL_STACK package offers a comprehensive API into the execution call stack, the error stack and the error backtrace. 

```sql
CREATE OR REPLACE PACKAGE pkg1
IS  
   PROCEDURE proc1;  
END pkg1; 
/
CREATE OR REPLACE PACKAGE BODY pkg1  
IS  
   PROCEDURE proc1  
   IS  
      PROCEDURE nested_in_proc1  
      IS  
      BEGIN  
        DBMS_OUTPUT.put_line ('*** "Traditional" Call Stack using FORMAT_CALL_STACK'); 
        DBMS_OUTPUT.put_line (DBMS_UTILITY.format_call_stack); 
  
        DBMS_OUTPUT.put_line ('*** Fully Qualified Nested Subprogram vis UTL_CALL_STACK');  
        DBMS_OUTPUT.put_line (  
           utl_call_stack.concatenate_subprogram (  
              utl_call_stack.subprogram (1)));  
      END;  
   BEGIN  
      nested_in_proc1;  
   END;  
END pkg1;
/

```

The FORMAT_CALL_STACK function in DBMS_UTILITY only shows you the name of the program unit in the call stack (i.e., the package name, *but not the function within the package*). 
UTL_CALL_STACK only shows you the name of the package subprogram, but even the name of nested (local) subprograms within those. 

```sql
BEGIN  
   pkg1.proc1;  
END; 
```

Take a look in the firs output line: *NESTED_IN_PROC1*

```
*** "Traditional" Call Stack using FORMAT_CALL_STACK
----- PL/SQL Call Stack -----
  object      line  frame       object
  handle    number  size        name
0x40706e6b8        11         416  package body SQL_RHMNFZMBVXMXUDWPPFNBLLUVE.PKG1.PROC1.NESTED_IN_PROC1
0x40706e6b8        21          24  package body SQL_RHMNFZMBVXMXUDWPPFNBLLUVE.PKG1.PROC1
0x20c994d60         2          24  anonymous block
0x45612b8e0      1721          88  package body SYS.DBMS_SQL.EXECUTE
0x465ed06e0      1365        4848  package body LIVESQL.ORACLE_SQL_EXEC.RUN_BLOCK
0x465ed06e0      1459         960  package body LIVESQL.ORACLE_SQL_EXEC.RUN_SQL
0x465ed06e0      1574         464  package body LIVESQL.ORACLE_SQL_EXEC.RUN_A_STATEMENT
0x465ed06e0      1898        5008  package body LIVESQL.ORACLE_SQL_EXEC.RUN_STATEMENTS
0x465ed06e0      2015        2528  package body LIVESQL.ORACLE_SQL_EXEC.RUN_STMTS
0x46e789230      2512        3712  package body LIVESQL.ORACLE_SQL_SCHEMA.RUN_SAVED_SESSION
0x45e286e48       341         112  package body LIVESQL.ORACLE_SQL_SCHEMA_PUB.RUN_SAVED_SESSION
0x407bde178        22        1616  anonymous block
0x45612b8e0      1721          88  package body SYS.DBMS_SQL.EXECUTE
0x417207590      1880         936  package body APEX_050100.WWV_FLOW_DYNAMIC_EXEC.RUN_BLOCK5
0x417207590       936         168  package body APEX_050100.WWV_FLOW_DYNAMIC_EXEC.EXECUTE_PLSQL_CODE
0x4858d9a38        71         256  package body APEX_050100.WWV_FLOW_PROCESS_NATIVE.PLSQL
0x4858d9a38      1132        4544  package body APEX_050100.WWV_FLOW_PROCESS_NATIVE.EXECUTE_PROCESS
0x466594bf0      2399        2744  package body APEX_050100.WWV_FLOW_PLUGIN.EXECUTE_PROCESS
0x46eabb2d0       200        2376  package body APEX_050100.WWV_FLOW_PROCESS.PERFORM_PROCESS
0x46eabb2d0       443       11744  package body APEX_050100.WWV_FLOW_PROCESS.PERFORM
0x4562256d0      4857          40  package body APEX_050100.WWV_FLOW.SHOW.RUN_BEFORE_HEADER_CODE
0x
*** Fully Qualified Nested Subprogram vis UTL_CALL_STACK
PKG1.PROC1.NESTED_IN_PROC1

```


