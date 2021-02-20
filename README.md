# I_am_learning_oracle19
##  sources from the web:
* oracle learning library - https://apexapps.oracle.com/pls/apex/f?p=44785:1:0
* oracle.com/plsql
* oracle livesql -https://livesql.oracle.com/
* ask Tom - ask_tom.oracle

## ACCESSIBLE BY Clause
https://oracle-base.com/articles/12c/plsql-white-lists-using-the-accessible-by-clause-12cr1
user1 is strong
user2 is week
we want to create a procedure that 

CREATE USER user1 IDENTIFIED BY test1;
GRANT CREATE SESSION, CREATE PROCEDURE, CREATE TABLE TO test1;

CREATE USER user2 IDENTIFIED BY test2;
GRANT CREATE SESSION, CREATE PROCEDURE TO test2;


CREATE OR REPLACE PROCEDURE protected_proc
  ACCESSIBLE BY (calling_proc)
AS
BEGIN
  DBMS_OUTPUT.put_line('TEST1 : protected_proc');
END;
/

## new data types

## optiomizing function

## UTL_CALL_STACK pacakge

## returrn resuilt set

## priveilages for progam units

## mark elemets for depracation

## plsql scope

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
 






