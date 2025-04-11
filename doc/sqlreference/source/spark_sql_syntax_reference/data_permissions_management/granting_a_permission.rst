:original_name: dli_08_0144.html

.. _dli_08_0144:

Granting a Permission
=====================

Function
--------

This statement is used to grant permissions to a user or role.

Syntax
------

::

   GRANT (privilege,...) ON (resource,..) TO ((ROLE [db_name].role_name) | (USER user_name)),...);

Keywords
--------

ROLE: The subsequent **role_name** must be a role.

USER: The subsequent **user_name** must be a user.

Precautions
-----------

-  The privilege must be one of the authorizable permissions. If the object has the corresponding permission on the resource or the upper-level resource, the permission fails to be granted. For details about the permission types supported by the privilege, see :ref:`Data Permissions List <dli_08_0140>`.
-  The resource can be a queue, database, table, view, or column. The formats are as follows:

   -  Queue format: queues.queue_name

      The following table lists the permission types supported by a queue.

      ================ =========================================
      Operation        Description
      ================ =========================================
      DROP_QUEUE       Deleting a queue
      SUBMIT_JOB       Submitting a job
      CANCEL_JOB       Cancel a job
      RESTART          Restarting a queue
      SCALE_QUEUE      Scaling out/in a queue
      GRANT_PRIVILEGE  Granting queue permissions
      REVOKE_PRIVILEGE Revoking queue permissions
      SHOW_PRIVILEGES  Checking queue permissions of other users
      ================ =========================================

   -  Database format: databases.db_name

      For details about the permission types supported by a database, see :ref:`Data Permissions List <dli_08_0140>`.

   -  Table format: databases.db_name.tables.table_name

      For details about the permission types supported by a table, see :ref:`Data Permissions List <dli_08_0140>`.

   -  View format: databases.db_name.tables.view_name

      Permission types supported by a view are the same as those supported by a table. For details, see table permissions in :ref:`Data Permissions List <dli_08_0140>`.

   -  Column format: databases.db_name.tables.table_name.columns.column_name

      Columns support only the SELECT permission.

Example
-------

Run the following statement to grant user\_name1 the permission to delete the **db1** database:

::

   GRANT DROP_DATABASE ON databases.db1 TO USER user_name1;

Run the following statement to grant user\_name1 the SELECT permission of data table **tb1** in the **db1** database:

::

   GRANT SELECT ON databases.db1.tables.tb1 TO USER user_name1;

Run the following statement to grant **role_name** the SELECT permission of data table **tb1** in the **db1** database:

::

   GRANT SELECT ON databases.db1.tables.tb1 TO ROLE role_name;
