:original_name: dli_01_0447.html

.. _dli_01_0447:

Managing Database Permissions
=============================

Scenario
--------

-  By setting permissions, you can assign varying database permissions to different users.
-  The administrator and database owner have all permissions, which cannot be set or modified by other users.

Precautions
-----------

-  Lower-level objects automatically inherit permissions granted to upper-level objects. The hierarchical relationship is database > table > column.

-  The database owner, table owner, and **authorized** users can assign permissions on the database and tables.

-  Columns can only inherit the query permission. For details about **Inheritable Permissions**, see :ref:`Managing Database Permissions <dli_01_0447>`.

-  The permissions can be revoked only at the initial level to which the permissions are granted. You need to grant and revoke permissions at the same level. You need to grant and revoke permissions at the same level. For example, after you are granted the insertion permission on a database, you can obtain the insertion permission on the tables in the database. Your insertion permission can be revoked only at the database level.

-  If you create a database with the same name as a deleted database, the database permissions will not be inherited. In this case, you need to grant the database permissions to users or projects.

   For example, user A is granted with the permission to delete the **testdb** database. Delete the database and create another one with the same name. You need to grant user A the deletion permission of the **testdb** database again.

Viewing Database Permissions
----------------------------

#. On the left of the management console, choose **Data Management** > **Databases and Tables**.

#. Locate the row where the target database resides and click **Manage Permissions** in the **Operation** column.

   Permissions can be granted to new users or projects, modified for users or projects with existing permissions, or revoked from a user or project.

Granting Permissions to a New User or Project
---------------------------------------------

Here, the new user or project refers to a user or a project that does not have permissions on the database.

#. Click a database you need. In the displayed **Database Permission Management** page, click **Grant Permission** in the upper right corner.

#. In the displayed dialog box, select **User** or **Project**, enter the username or select the project to be authorized, and select the required permissions. For details about the permissions, see :ref:`Table 1 <dli_01_0447__table88751410195512>`.

   .. _dli_01_0447__table88751410195512:

   .. table:: **Table 1** Parameters

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                              |
      +===================================+==========================================================================================================================================================================+
      | Authorization Object              | Select **User** or **Project**.                                                                                                                                          |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Username/Project Name             | -  If you select **User**, enter the IAM username when adding a user to the database.                                                                                    |
      |                                   |                                                                                                                                                                          |
      |                                   |    .. note::                                                                                                                                                             |
      |                                   |                                                                                                                                                                          |
      |                                   |       The username is an existing IAM user name and has logged in to the DLI management console.                                                                         |
      |                                   |                                                                                                                                                                          |
      |                                   | -  If you select **Project**, select the project to be authorized in the current region.                                                                                 |
      |                                   |                                                                                                                                                                          |
      |                                   |    .. note::                                                                                                                                                             |
      |                                   |                                                                                                                                                                          |
      |                                   |       When **Project** is selected:                                                                                                                                      |
      |                                   |                                                                                                                                                                          |
      |                                   |       -  If you set **Non-inheritable Permissions**, you cannot view tables in the corresponding database within the project.                                            |
      |                                   |       -  If you set **Inheritable Permissions**, you can view all tables in the database within the project.                                                             |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Non-Inherited Permissions         | Select a permission to grant it to the user, or deselect a permission to revoke it.                                                                                      |
      |                                   |                                                                                                                                                                          |
      |                                   | Non-inherited permissions apply only to the current database.                                                                                                            |
      |                                   |                                                                                                                                                                          |
      |                                   | -  The following permissions are applicable to both user and project authorization:                                                                                      |
      |                                   |                                                                                                                                                                          |
      |                                   |    -  **Drop Database**: This permission allows you to delete the current database.                                                                                      |
      |                                   |    -  **Create Table**: This permission allows you to create tables in the current database.                                                                             |
      |                                   |    -  **Create View**: This permission allows you to create views in the current database.                                                                               |
      |                                   |    -  **Execute SQL EXPLAIN**: This permission allows you to execute an EXPLAIN statement and view information about how this database executes a query.                 |
      |                                   |    -  **Create Role**: This permission allows you to create roles in the current database.                                                                               |
      |                                   |    -  **Delete Role**: This permission allows you to delete roles of the current database.                                                                               |
      |                                   |    -  **View Role**: This permission allows you to view the role of the current user.                                                                                    |
      |                                   |    -  **Bind Role**: This permission allows you to bind roles to the current database.                                                                                   |
      |                                   |    -  **Unbind Role**: This permission allows you to bind roles from the current database.                                                                               |
      |                                   |    -  **View All Binding Relationships**: This permission allows you to view the binding relationships between all roles and users.                                      |
      |                                   |    -  **Create Function**: This permission allows you to create a function in the current database.                                                                      |
      |                                   |    -  **Delete Function**: This permission allows you to delete functions from the current database.                                                                     |
      |                                   |    -  **View All Functions**: This permission allows you to view all functions in the current database.                                                                  |
      |                                   |    -  **View Function Details**: This permission allows you to view details about the current function.                                                                  |
      |                                   |                                                                                                                                                                          |
      |                                   | -  The following permissions can only be granted to users:                                                                                                               |
      |                                   |                                                                                                                                                                          |
      |                                   |    -  **View All Tables**: This permission allows you to view all tables in the current database.                                                                        |
      |                                   |                                                                                                                                                                          |
      |                                   |       .. note::                                                                                                                                                          |
      |                                   |                                                                                                                                                                          |
      |                                   |          If this permission of a specific database is not granted, all tables in the database will not be displayed.                                                     |
      |                                   |                                                                                                                                                                          |
      |                                   |    -  **View Database**: This permission allows you to view the information about the current database.                                                                  |
      |                                   |                                                                                                                                                                          |
      |                                   |       .. note::                                                                                                                                                          |
      |                                   |                                                                                                                                                                          |
      |                                   |          If this permission is not granted, the database will not be displayed.                                                                                          |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Inherited Permissions             | Select a permission to grant it to the user, or deselect a permission to revoke it.                                                                                      |
      |                                   |                                                                                                                                                                          |
      |                                   | Inherited permissions are applicable to the current database and all its tables. However, only the query permission is applicable to table columns.                      |
      |                                   |                                                                                                                                                                          |
      |                                   | The following permissions can be granted to both user and project.                                                                                                       |
      |                                   |                                                                                                                                                                          |
      |                                   | -  **Drop Table**: This permission allows you to delete tables in a database.                                                                                            |
      |                                   | -  **Select Table**: This permission allows you to query data of the current table.                                                                                      |
      |                                   | -  **View Table Information**: This permission allows you to view information about the current table.                                                                   |
      |                                   | -  **Insert**: This permission allows you to insert data into the current table.                                                                                         |
      |                                   | -  **Add Column**: This permission allows you to add columns to the current table.                                                                                       |
      |                                   | -  **Overwrite**: This permission allows you to insert data to overwrite the data in the current table.                                                                  |
      |                                   | -  **Grant Permission**: This permission allows you to grant database permissions to other users or projects.                                                            |
      |                                   | -  **Revoke Permission**: This permission allows you to revoke the permissions of the database that other users have but cannot revoke the database owner's permissions. |
      |                                   | -  **Add Partition to Partition Table**: This permission allows you to add a partition to a partition table.                                                             |
      |                                   | -  **Delete Partition from Partition Table**: This permission allows you to delete existing partitions from a partition table.                                           |
      |                                   | -  **Configure Path for Partition**: This permission allows you to set the path of a partition in a partition table to a specified OBS path.                             |
      |                                   | -  **Rename Table Partition**: This permission allows you to rename partitions in a partition table.                                                                     |
      |                                   | -  **Rename Table**: This permission allows you to rename tables.                                                                                                        |
      |                                   | -  **Restore Table Partition**: This permission allows you to export partition information from the file system and save the information to metadata.                    |
      |                                   | -  **View All Partitions**: This permission allows you to view all partitions in a partition table.                                                                      |
      |                                   | -  **View Other Users' Permissions**: This permission allows you to query other users' permission on the current database.                                               |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

Modifying Permissions for an Existing User or Project
-----------------------------------------------------

For a user or project that has some permissions on the database, you can revoke the existing permissions or grant new ones.

.. note::

   If the options in **Set Permission** are gray, the corresponding account does not have the permission to modify the database. You can apply to the administrator, database owner, or other authorized users for granting and revoking permissions of databases.

#. In the **User Permission Info** list, find the user whose permission needs to be set.

   -  If the user is a sub-user, you can set permissions for it.
   -  If the user is already an administrator, you can only view the permissions information.

   In the **Project Permission Info** list, locate the project for which you want to set permissions and click **Set Permission**.

#. In the **Operation** column of the sub-user or project, click **Set Permission**. The **Set Permission** dialog box is displayed.

   For details about the permissions of database users or projects, see :ref:`Table 1 <dli_01_0447__table88751410195512>`.

#. Click **OK**.

Revoking All Permissions of a User or Project
---------------------------------------------

Revoke all permissions of a user or a project.

-  In the user list under **User Permission Info**, locate the row where the target sub-user resides and click **Revoke Permission** in the **Operation** column. In the displayed dialog box, click **OK**. In this case, the user has no permissions on the database.

   .. note::

      If a user is an administrator, **Revoke Permission** is gray, indicating that the user's permission cannot be revoked.

-  In the **Project Permission Info** area, select the project whose permissions need to be revoked and click **Revoke Permission** in the **Operation** column. After you click **OK**, the project does not have any permissions on the database.
