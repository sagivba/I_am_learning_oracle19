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

## oracle record contractors
https://livesql.oracle.com/apex/livesql/file/content_F9WWD55FZB0LPDH74V0NVBSHU.html
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





 






