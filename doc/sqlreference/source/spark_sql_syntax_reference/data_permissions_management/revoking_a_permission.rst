:original_name: dli_08_0146.html

.. _dli_08_0146:

Revoking a Permission
=====================

Function
--------

This statement is used to revoke permissions granted to a user or role.

Syntax
------

::

   REVOKE (privilege,...) ON (resource,..) FROM ((ROLE [db_name].role_name) | (USER user_name)),...);

Keyword
-------

ROLE: The subsequent **role_name** must be a role.

USER: The subsequent **user_name** must be a user.

Precautions
-----------

-  The privilege must be the granted permissions of the authorized object in the resource. Otherwise, the permission fails to be revoked. For details about the permission types supported by the privilege, see :ref:`Data Permissions List <dli_08_0140>`.
-  The resource can be a queue, database, table, view, or column. The formats are as follows:

   -  Queue format: queues.queue_name
   -  Database format: databases.db_name
   -  Table format: databases.db_name.tables.table_name
   -  View format: databases.db_name.tables.view_name
   -  Column format: databases.db_name.tables.table_name.columns.column_name

Example
-------

To revoke the permission of user **user_name1** to delete database **db1**, run the following statement:

::

   REVOKE DROP_DATABASE ON databases.db1 FROM USER user_name1;

To revoke the SELECT permission of user **user_name1** on table **tb1** in database **db1**, run the following statement:

::

   REVOKE SELECT ON databases.db1.tables.tb1 FROM USER user_name1;

To revoke the SELECT permission of role **role_name** on table **tb1** in database **db1**, run the following statement:

::

   REVOKE SELECT ON databases.db1.tables.tb1 FROM ROLE role_name;
