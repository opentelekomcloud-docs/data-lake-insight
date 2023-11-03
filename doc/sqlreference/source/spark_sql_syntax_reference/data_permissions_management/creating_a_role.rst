:original_name: dli_08_0141.html

.. _dli_08_0141:

Creating a Role
===============

Function
--------

-  This statement is used to create a role in the current database or a specified database.
-  Only users with the CREATE_ROLE permission on the database can create roles. For example, the administrator, database owner, and other users with the CREATE_ROLE permission.
-  Each role must belong to only one database.

Syntax
------

::

   CREATE ROLE [db_name].role_name;

Keyword
-------

None

Precautions
-----------

-  The **role_name** to be created must not exist in the current database or the specified database. Otherwise, an error will be reported.
-  If **db_name** is not specified, the role is created in the current database.

Example
-------

::

   CREATE ROLE role1;
