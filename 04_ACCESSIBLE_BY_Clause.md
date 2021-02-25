# ACCESSIBLE BY Clause
* [oracle doc](http://docs.oracle.com/database/122/LNPLS/ACCESSIBLE-BY-clause.htm)
* [oracle-base](https://oracle-base.com/articles/12c/plsql-white-lists-using-the-accessible-by-clause-12cr1)
* [livesql](https://livesql.oracle.com/apex/livesql/file/content_EF5CJBRFPTA5PLU85RB6FS2FC.html)


The ACCESSIBLE BY clause restricts access to units and subprograms by other units.
The accessor list, also known as the white list, explicitly lists those units which may have access. 
In 12.2, this feature was enhanced to be applied to subprograms in packages.
You can also specify the type or "unit kind" to which the whitelist applies. 
This is useful when you have triggers and PL/SQL program units of the same name. 

## Apply ACCESSIBLE BY To Subprograms and Specify "Unit Kind"
*the ACCESSIBLE BY clauses in the package specification must be repeated in the body.*

```sql
CREATE OR REPLACE PACKAGE protected_pkg 
IS 
   PROCEDURE do_this; 
   PROCEDURE this_for_proc_only        ACCESSIBLE BY (PROCEDURE generic_name); 
   PROCEDURE this_for_trigger_only     ACCESSIBLE BY (TRIGGER generic_name); 
   PROCEDURE this_for_any_generic_name ACCESSIBLE BY (generic_name); 
 
   /* This will not work - only program units can be named, not subprograms */ 
   -- PROCEDURE this_for_pkgd_proc1_only 
   --   ACCESSIBLE BY (PROCEDURE pkg1.myproc1); 
END;
/
CREATE OR REPLACE PACKAGE BODY protected_pkg
IS
   PROCEDURE do_this IS
   BEGIN NULL;   END;

   PROCEDURE this_for_proc_only ACCESSIBLE BY (PROCEDURE generic_name) IS
   BEGIN NULL;   END;

   PROCEDURE this_for_trigger_only ACCESSIBLE BY (TRIGGER generic_name) IS
   BEGIN NULL;   END;

   PROCEDURE this_for_any_generic_name ACCESSIBLE BY (generic_name) IS
   BEGIN NULL;   END;

   PROCEDURE this_for_pkgd_proc1_only ACCESSIBLE BY (PROCEDURE pkg1.myproc1) IS
   BEGIN NULL;   END;
END;
/
```

## Procedure Invokes Procedure-only Procedure

```sql
CREATE OR REPLACE PROCEDURE generic_name   AUTHID DEFINER IS 
BEGIN 
   pkg.this_for_proc_only; 
END;
/
Procedure created.
```

## Procedure Invokes Trigger-only Procedure

```sql
CREATE OR REPLACE PROCEDURE generic_name  AUTHID DEFINER IS 
BEGIN 
   pkg.this_for_trigger_only; 
END;
/
```

*Errors:* PROCEDURE GENERIC_NAME
Line: 5 PLS-00904: insufficient privilege to access object THIS_FOR_TRIGGER_ONLY

## we can not run these the protected procedures

```sql
exec protected_pkg.this_for_proc_only
```
ORA-06550: line 1, column 7:
PLS-00904: insufficient privilege to access object PRIVATE_PROC 


