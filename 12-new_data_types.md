# new data types

Staring 12.1 vertion oracle  defined several new  data types:

## BINARY_FLOAT Data Type
There are two Oracle data types for storing floating-point numbers. One is BINARY_FLOAT, and the other is BINARY_DOUBLE which is detailed below.

The difference between BINARY_FLOAT and number is:

NUMBER uses decimal precision and BINARY_FLOAT uses binary precision.
BINARY_FLOAT can perform arithmetic calculations faster and is usually smaller to store.
NUMBER is an exact definition, whereas BINARY_FLOAT is an approximate definition.
The main difference between these two types is the third point – BINARY_FLOAT is approximate. This means the data type stores approximate representations of decimal values, rather than the exact value.

It’s similar to a FLOAT data type in other languages. BINARY_FLOAT is a 32-bit single precision value.

### When to use BINARY_FLOAT
If performance is important, or if you need to store a large floating-point number.
### converting from number to BINARY_FLOAT
The function `TO_BINARY_FLOAT` returns a single-precision floating-point number.
 

## BINARY_DOUBLE Data Type
Just like the BINARY_FLOAT data type, the BINARY_DOUBLE stores approximate values rather than exact values, and is faster at calculations than NUMBER.

However, because it doesn’t store exact values, it has a specialised need. It works like a DOUBLE in other languages.

The difference between BINARY_FLOAT and BINARY_DOUBLE is that BINARY_DOUBLE is that the BINARY_DOUBLE is a 64-bit double precision value.

### When to use BINARY_DOUBLE
If performance is important, or if you need to store a floating-point number larger than what BINARY_FLOAT can handle.

### converting from number to BINARY_DOUBLE 
The function TO_BINARY_DOUBLE returns a double-precision floating-point number.

## PLS_INTEGER Data Type
The PLS_INTEGER data type is only available to PL/SQL code. It works in a similar way to the NUMBER value in Oracle SQL.

PLS_INTEGER is an integer data type, which means it can only store whole numbers. It’s signed, which means it can store positive and negative numbers.

The range is from -2,147,483,647 to 2,147,483,647.

If you’re using PL/SQL, you can use this data type. They perform better and require less storage than NUMBER data types.

### When to use PLS_INTEGER
If you’re using PL/SQL and you need to use integer values.
Use PLS_INTEGER instead of BINARY_INTEGER which is older, and slower version of the PLS_INTEGER. 



