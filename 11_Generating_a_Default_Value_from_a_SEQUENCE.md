# Generating a Default Value from a SEQUENCE
Referencing a sequence as a column default value in a create table statement. New with database 12c.

```sql
CREATE TABLE db_12c_style_identity 
(  
id               INTEGER  DEFAULT ON NULL db_id_test_seq.nextval PRIMARY KEY, 
another_column   VARCHAR2(30) 
)
```

