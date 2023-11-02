:original_name: dli_08_0142.html

.. _dli_08_0142:

Binding a Role
==============

Function
--------

This statement is used to bind a user with a role.

Syntax
------

::

   GRANT ([db_name].role_name,...) TO (user_name,...);

Keyword
-------

None

Precautions
-----------

The **role_name** and **username** must exist. Otherwise, an error will be reported.

Example
-------

::

   GRANT role1 TO user_name1;
