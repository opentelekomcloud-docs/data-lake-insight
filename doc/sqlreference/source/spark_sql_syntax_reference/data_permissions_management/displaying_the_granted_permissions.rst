:original_name: dli_08_0145.html

.. _dli_08_0145:

Displaying the Granted Permissions
==================================

Function
--------

This statement is used to show the permissions granted to a user or role in the resource.

Syntax
------

::

   SHOW GRANT ((ROLE [db_name].role_name) | (USER user_name)) ON resource;

Keyword
-------

ROLE: The subsequent **role_name** must be a role.

USER: The subsequent **user_name** must be a user.

Precautions
-----------

The resource can be a queue, database, table, view, or column. The formats are as follows:

-  Queue format: queues.queue_name
-  Database format: databases.db_name
-  Table format: databases.db_name.tables.table_name
-  Column format: databases.db_name.tables.table_name.columns.column_name
-  View format: databases.db_name.tables.view_name

Example
-------

Run the following statement to show permissions of **user_name1** in the **db1** database:

::

   SHOW GRANT USER user_name1 ON databases.db1;

Run the following statement to show permissions of **role_name** on table **tb1** in the **db1** database:

::

   SHOW GRANT ROLE role_name ON databases.db1.tables.tb1;
