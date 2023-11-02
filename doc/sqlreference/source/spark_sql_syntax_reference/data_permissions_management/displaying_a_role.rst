:original_name: dli_08_0143.html

.. _dli_08_0143:

Displaying a Role
=================

Function
--------

This statement is used to display all roles or roles bound to the **user_name** in the current database.

Syntax
------

::

   SHOW [ALL] ROLES [user_name];

Keyword
-------

ALL: Displays all roles.

Precautions
-----------

Keywords ALL and user_name cannot coexist.

Example
-------

-  To display all roles bound to the user, run the following statement:

   ::

      SHOW ROLES;

-  To display all roles in the project, run the following statement:

   ::

      SHOW ALL ROLES;

   .. note::

      Only the administrator has the permission to run the **show all roles** statement.

-  To display all roles bound to the user named **user_name1**, run the following statement:

   ::

      SHOW ROLES user_name1;
