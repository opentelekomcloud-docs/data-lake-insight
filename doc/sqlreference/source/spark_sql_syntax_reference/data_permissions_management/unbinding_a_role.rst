:original_name: dli_08_0147.html

.. _dli_08_0147:

Unbinding a Role
================

Function
--------

This statement is used to unbind the user with the role.

Syntax
------

::

   REVOKE ([db_name].role_name,...) FROM (user_name,...);

Keywords
--------

None

Precautions
-----------

role_name and user_name must exist and user_name has been bound to role_name.

Example
-------

To unbind the user_name1 from role1, run the following statement:

::

   REVOKE role1 FROM user_name1;
