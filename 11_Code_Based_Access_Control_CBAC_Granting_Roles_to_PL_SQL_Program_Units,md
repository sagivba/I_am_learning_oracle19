# Code Based Access Control (CBAC) : Granting Roles to PL/SQL Program Units
https://oracle-base.com/articles/12c/code-based-access-control-12cr1#:~:text=Oracle%2012c%20introduced%20code%20based,objects%20directly%20to%20that%20user.
By default, PL/SQL program units are created using definer rights.
So they are executed with all the privileges granted directly to the user that created them. 
This can be very useful when you want low privileged users to perform tasks that require a high level of privilege.
 this is implemented by wrapping PL/SQL program unit, with execute privilege granted to the low privileged user. 
 
 The problem with definer rights is it is very easy to accidentally expose excessive functionality to a user.

An alternative is to create the program unit with invoker rights, so it is run in the context the calling user, rather than the user that created it.
The advantage of this is the program unit is only able to perform tasks that the calling user has privilege to perform, including those privileges granted via roles.
Invoker rights has a number of issues.
ode based access control (CBAC), allowing roles to be granted directly to definer and invoker rights program units, thereby letting you to guarantee the level of privilege present in the calling user, without having to expose additional objects directly to that user. 

