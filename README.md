# I_am_learning_oracle19
##  sources from the web:
* oracle learning library - https://apexapps.oracle.com/pls/apex/f?p=44785:1:0
* oracle.com/plsql
* oracle livesql -https://livesql.oracle.com/
* ask Tom - ask_tom.oracle

## ACCESSIBLE BY Clause
Doc: http://docs.oracle.com/database/122/LNPLS/ACCESSIBLE-BY-clause.htm
https://oracle-base.com/articles/12c/plsql-white-lists-using-the-accessible-by-clause-12cr1
https://livesql.oracle.com/apex/livesql/file/content_EF5CJBRFPTA5PLU85RB6FS2FC.html


The ACCESSIBLE BY clause restricts access to units and subprograms by other units. The accessor list, also known as the white list, explicitly lists those units which may have access. In 12.2, this feature was enhanced to be applied to subprograms in packages. You can also specify the type or "unit kind" to which the whitelist applies. This is useful when you have triggers and PL/SQL program units of the same name. 

### Apply ACCESSIBLE BY To Subprograms and Specify "Unit Kind"
*the ACCESSIBLE BY clauses in the package specification must be repeated in the body.*

CREATE OR REPLACE PACKAGE protected_pkg 
   AUTHID DEFINER 
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
   BEGIN
      NULL;
   END;

   PROCEDURE this_for_proc_only ACCESSIBLE BY (PROCEDURE generic_name) IS
   BEGIN
      NULL;
   END;

   PROCEDURE this_for_trigger_only ACCESSIBLE BY (TRIGGER generic_name) IS
   BEGIN
      NULL;
   END;

   PROCEDURE this_for_any_generic_name ACCESSIBLE BY (generic_name) IS
   BEGIN
      NULL;
   END;

   PROCEDURE this_for_pkgd_proc1_only ACCESSIBLE BY (PROCEDURE pkg1.myproc1) IS
   BEGIN
      NULL;
   END;
END;
/

### Procedure Invokes Procedure-only Procedure
CREATE OR REPLACE PROCEDURE generic_name   AUTHID DEFINER IS 
BEGIN 
   pkg.this_for_proc_only; 
END;
/
Procedure created.

### Procedure Invokes Trigger-only Procedure

CREATE OR REPLACE PROCEDURE generic_name  AUTHID DEFINER IS 
BEGIN 
   pkg.this_for_trigger_only; 
END;
/

Errors: PROCEDURE GENERIC_NAME
Line: 5 PLS-00904: insufficient privilege to access object THIS_FOR_TRIGGER_ONLY

### we can not run these the protected procedures
exec protected_pkg.this_for_proc_only
ORA-06550: line 1, column 7:
PLS-00904: insufficient privilege to access object PRIVATE_PROC 


## new data types

## optiomizing function
### UDF pragma 
https://mwidlake.wordpress.com/2015/11/04/pragma-udf-speeding-up-your-plsql-functions-called-from-sql/
The UDF pragma tells the compiler that the PL/SQL unit is a user defined function that is used primarily in SQL statements, which might improve its performance.

As of Oracle Database 12c, there is also the possibility of adding a PL/SQL function to your SQL statement with the ```WITH``` clause. 
A non-trivial example is described on Databaseline, from which it follows that the WITH clause is marginally faster than the UDF pragma, 
but that the latter has the advantage that it is modular, whereas the former is the equivalent of hard coding your functions.

We can therefore recommend that you first try to add ```PRAGMA UDF``` to your PL/SQL functions if and only if they are called from SQL statements but not PL/SQL code. 
If that does not provide a significant benefit, then try the WITH function block.

## UTL_CALL_STACK pacakge

## returrn resuilt set

## priveilages for progam units

## mark elemets for depracation

## plsql scope

## Generating a Default Value from a SEQUENCE
Referencing a sequence as a column default value in a create table statement. New with database 12c.
create table db_12c_style_identity 
(  
id               integer  DEFAULT ON NULL db_id_test_seq.nextval primary key, 
another_column   varchar2(30) 
)

## fetch first X rows only
now we you can limit your SQL query result sets to a specified number of rows.
 “fetch first N” turns into a hidden row_number() over() analytic function - so use “fetch first N rows” instead of “where rownum <= N” 


select * from hr.employees order by last_name fetch first 4 rows only



## oracle record contractors (Qualified Expressions)
https://livesql.oracle.com/apex/livesql/file/content_F9WWD55FZB0LPDH74V0NVBSHU.html
https://livesql.oracle.com/apex/livesql/file/content_GAE2LUPS0UA1IU1SUIAZCB7W1.html

Starting with Oracle Database Release 18c, any PL/SQL value can be provided by an expression (for example for a record or for an associative array) like a constructor provides an abstract datatype value. In PL/SQL, we use the terms "qualified expression" and "aggregate" rather than the SQL term "type constructor", but the functionality is the same. Qualified expressions improve program clarity and developer productivity by providing the ability to declare and define a complex value in a compact form where the value is needed. A qualified expression combines expression elements to create values of a RECORD type or associative array type. Qualified expressions use an explicit type indication to provide the type of the qualified item. This explicit indication is known as a typemark.

DECLARE 
   TYPE species_rt IS RECORD 
   ( 
      species_name           VARCHAR2 (100), 
      habitat_type           VARCHAR2 (100), 
      surviving_population   INTEGER 
   ); 
 
   l_elephant   species_rt := species_rt ('Elephant', 'Savannah', '10000'); 
 
   PROCEDURE display_species ( 
      species_in    species_rt DEFAULT species_rt ('Not Set', 'Global', 0)) 
   IS 
   BEGIN 
      DBMS_OUTPUT.put_line ('Species: ' || species_in.species_name); 
      DBMS_OUTPUT.put_line ('Habitat: ' || species_in.habitat_type); 
      DBMS_OUTPUT.put_line ('# Left: ' || species_in.surviving_population); 
   END; 
BEGIN 
   display_species (l_elephant); 
 
   /* Use the default */ 
   display_species (); 
END;

Now we  can "call a function" (qualified expression - like OOP contractor ) with the same name as your record type and pass values for our fields inside it. 
This example uses positional notation to associate values with fields. 
Notice also that I use the qualified expression as a default value for my parameter!
### Qualified Expressions for Associative Arrays
Any PL/SQL value can be provided by an expression (for example for a *record* or for an *associative array*) like a constructor provides an abstract datatype value. 
In PL/SQL, ORACLE are using the terms "qualified expression" and "aggregate" rather than the SQL term "type constructor" (and I don not aunderstand why...).
The functionality is the same. 
Qualified expressions provids the ability to declare and define a complex value in a compact form where the value is needed. 
This can help to improve program clarity and developer productivity.

A qualified expression combines expression elements to create values of a RECORD type or associative array type. 
Qualified expressions use an explicit type indication to provide the type of the qualified item. 
This explicit indication is known as a typemark.
#### Before:
DECLARE   
   TYPE ints_t IS TABLE OF INTEGER   
      INDEX BY PLS_INTEGER;   
   
   l_ints   ints_t;   
BEGIN   
   l_ints (1) := 55;  
   l_ints (2) := 555;  
   l_ints (3) := 5555;  
  
   FOR indx IN 1 .. l_ints.COUNT   
   LOOP   
      DBMS_OUTPUT.put_line (l_ints (indx));   
   END LOOP;   
END;

#### And Now:
DECLARE  
   TYPE ints_t IS TABLE OF INTEGER  
      INDEX BY PLS_INTEGER;  
  
   l_ints   ints_t := ints_t (1 => 55, 2 => 555, 3 => 5555);  
BEGIN  
   FOR indx IN 1 .. l_ints.COUNT  
   LOOP  
      DBMS_OUTPUT.put_line (l_ints (indx));  
   END LOOP;  
END;

##### using the species_rt above:

DECLARE
   TYPE species_rt IS RECORD
   (
      species_name VARCHAR2 (100),
      habitat_type VARCHAR2 (100),
      surviving_population INTEGER
   );
   
   TYPE species_t IS TABLE OF species_rt
      INDEX BY PLS_INTEGER;

   l_species   species_t := 
      species_t (
         2 => species_rt ('Elephant', 'Savannah', '10000'), 
         1 => species_rt ('Dodos', 'Mauritius', '0'), 
         3 => species_rt ('Venus Flytrap', 'North Carolina', '250'));
BEGIN
   FOR indx IN 1 .. l_species.COUNT
   LOOP
      DBMS_OUTPUT.put_line (l_species (indx).species_name);
   END LOOP;
END;



##### Qualified Expressions for String-Indexed Arrays

DECLARE 
   TYPE by_string_t IS TABLE OF INTEGER 
      INDEX BY VARCHAR2(100); 
 
   l_stuff   by_string_t := by_string_t ('Steven' => 55, 'Loey' => 555, 'Juna' => 5555); 
   l_index varchar2(100) := l_stuff.first; 
BEGIN 
   DBMS_OUTPUT.put_line (l_stuff.count); 
    
   WHILE l_index IS NOT NULL 
   LOOP 
      DBMS_OUTPUT.put_line (l_index || ' => ' || l_stuff (l_index)); 
      l_index := l_stuff.NEXT (l_index); 
   END LOOP; 
END;
 






