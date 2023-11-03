:original_name: dli_08_0148.html

.. _dli_08_0148:

Deleting a Role
===============

Function
--------

This statement is used to delete a role in the current database or a specified database.

Syntax
------

::

   DROP ROLE [db_name].role_name;

Keyword
-------

None

Precautions
-----------

-  The **role_name** to be deleted must exist in the current database or the specified database. Otherwise, an error will be reported.
-  If **db_name** is not specified, the role is deleted in the current database.

Example
-------

::

   DROP ROLE role1;
