:original_name: dli_01_0448.html

.. _dli_01_0448:

Configuring Table Permissions on the DLI Console
================================================

Operation Scenario
------------------

-  By setting permissions, you can assign varying table permissions to different users.
-  The administrator and table owner have all permissions, which cannot be set or modified by other users.
-  When setting table permissions for a new user, ensure that the user group the user belongs to has the **Tenant Guest** permission.

Precautions
-----------

-  If you create a table with the same name as a deleted table, the table permissions will not be inherited. In this case, you need to grant the table permissions to users or projects.

   For example, user A is granted with the permission to delete the **testTable** table. Delete the table and create another one with the same name. You need to grant user A the deletion permission of the **testTable** table again.

Viewing Table Permissions
-------------------------

#. On the left of the management console, choose **Data Management** > **Databases and Tables**.

#. Click the database name in the table whose authority is to be set. The **Table Management** page of the database is displayed.

#. Locate the row where the target table resides and click **Manage Permissions** in the **Operation** column.

   Permissions can be granted to new users or projects, modified for users or projects with existing permissions, or revoked from a user or project.

Granting Permissions to a New User or a Project
-----------------------------------------------

Here, the new user or project refers to a user or a project that does not have permissions on the database.

#. Click the table you need. In the displayed table permissions page, click **Grant Permission** in the upper right corner.
#. In the displayed **Grant Permission** dialog box, select the required permissions.

   -  For details about the DLI table permissions, see :ref:`Table 1 <dli_01_0448__table06511040124817>`.

      .. _dli_01_0448__table06511040124817:

      .. table:: **Table 1** Parameter description

         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                             |
         +===================================+=========================================================================================================================================================+
         | Authorization Object              | Select **User** or **Project**.                                                                                                                         |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Username/Project                  | -  If you select **User**, enter the IAM username when granting table permissions to the user.                                                          |
         |                                   |                                                                                                                                                         |
         |                                   |    .. note::                                                                                                                                            |
         |                                   |                                                                                                                                                         |
         |                                   |       The username is an existing IAM user name and has logged in to the DLI management console.                                                        |
         |                                   |                                                                                                                                                         |
         |                                   | -  If you select **Project**, select the project to be authorized in the current region.                                                                |
         |                                   |                                                                                                                                                         |
         |                                   |    .. note::                                                                                                                                            |
         |                                   |                                                                                                                                                         |
         |                                   |       If you select **Project**, you can only view information about the authorized tables and their databases.                                         |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Non-inheritable Permissions       | Select a permission to grant it to the user, or deselect a permission to revoke it.                                                                     |
         |                                   |                                                                                                                                                         |
         |                                   | -  The following permissions are applicable to both user and project authorization:                                                                     |
         |                                   |                                                                                                                                                         |
         |                                   |    -  **Select Table**: This permission allows you to query data of the current table.                                                                  |
         |                                   |    -  **View Table Information**: This permission allows you to view information about the current table.                                               |
         |                                   |    -  **View Table Creation Statement**: This permission allows you to view the statement for creating the current table.                               |
         |                                   |    -  **Drop Table**: This permission allows you to delete the current table.                                                                           |
         |                                   |    -  **Rename Table**: Rename the current table.                                                                                                       |
         |                                   |    -  **Insert**: This permission allows you to insert data into the current table.                                                                     |
         |                                   |    -  **Overwrite**: This permission allows you to insert data to overwrite the data in the current table.                                              |
         |                                   |    -  **Add Column**: Add columns to the current table.                                                                                                 |
         |                                   |    -  **Grant Permission**: The current user can grant table permissions to other users.                                                                |
         |                                   |    -  **Revoke Permission**: The current user can revoke the table's permissions that other users have but cannot revoke the table owner's permissions. |
         |                                   |    -  **View Other Users' Permissions**: This permission allows you to query other users' permission on the current table.                              |
         |                                   |                                                                                                                                                         |
         |                                   |    The partition table also has the following permissions:                                                                                              |
         |                                   |                                                                                                                                                         |
         |                                   |    -  **Delete Partition**: This permission allows you to delete existing partitions from a partition table.                                            |
         |                                   |    -  **View All Partitions**: This permission allows you to view all partitions in a partition table.                                                  |
         |                                   |                                                                                                                                                         |
         |                                   | -  The following permissions can only be granted to users:                                                                                              |
         |                                   |                                                                                                                                                         |
         |                                   |    -  **View Table**: This permission allows you to display the current table.                                                                          |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  For details about the OBS table permissions, see :ref:`Table 2 <dli_01_0448__table36563404488>`.

      .. _dli_01_0448__table36563404488:

      .. table:: **Table 2** Parameter description

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                  |
         +===================================+==============================================================================================================================================================================+
         | Authorization Object              | Select **User** or **Project**.                                                                                                                                              |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Username/Project                  | -  If you select **User**, enter the IAM username when granting table permissions to the user.                                                                               |
         |                                   |                                                                                                                                                                              |
         |                                   |    .. note::                                                                                                                                                                 |
         |                                   |                                                                                                                                                                              |
         |                                   |       The username is an existing IAM user name and has logged in to the DLI management console.                                                                             |
         |                                   |                                                                                                                                                                              |
         |                                   | -  If you select **Project**, select the project to be authorized in the current region.                                                                                     |
         |                                   |                                                                                                                                                                              |
         |                                   |    .. note::                                                                                                                                                                 |
         |                                   |                                                                                                                                                                              |
         |                                   |       If you select **Project**, you can only view information about the authorized tables and their databases.                                                              |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Non-inheritable Permissions       | Select a permission to grant it to the user, or deselect a permission to revoke it.                                                                                          |
         |                                   |                                                                                                                                                                              |
         |                                   | -  The following permissions are applicable to both user and project authorization:                                                                                          |
         |                                   |                                                                                                                                                                              |
         |                                   |    -  **View Table Creation Statement**: This permission allows you to view the statement for creating the current table.                                                    |
         |                                   |    -  **View Table Information**: This permission allows you to view information about the current table.                                                                    |
         |                                   |    -  **Select Table**: This permission allows you to query data of the current table.                                                                                       |
         |                                   |    -  **Drop Table**: This permission allows you to delete the current table.                                                                                                |
         |                                   |    -  **Rename Table**: Rename the current table.                                                                                                                            |
         |                                   |    -  **Insert**: This permission allows you to insert data into the current table.                                                                                          |
         |                                   |    -  **Overwrite**: This permission allows you to insert data to overwrite the data in the current table.                                                                   |
         |                                   |    -  **Add Column**: This permission allows you to add columns to the current table.                                                                                        |
         |                                   |    -  **Grant Permission**: This permission allows you to grant table permissions to other users or projects.                                                                |
         |                                   |    -  **Revoke Permission**: This permission allows you to revoke the table's permissions that other users or projects have but cannot revoke the table owner's permissions. |
         |                                   |    -  **View Other Users' Permissions**: This permission allows you to query other users' permission on the current table.                                                   |
         |                                   |                                                                                                                                                                              |
         |                                   |    The partition table also has the following permissions:                                                                                                                   |
         |                                   |                                                                                                                                                                              |
         |                                   |    -  **Add Partition**: This permission allows you to add a partition to a partition table.                                                                                 |
         |                                   |    -  **Delete Partition**: This permission allows you to delete existing partitions from a partition table.                                                                 |
         |                                   |    -  **Configure Path for Partition**: This permission allows you to set the path of a partition in a partition table to a specified OBS path.                              |
         |                                   |    -  **Rename Table Partition**: This permission allows you to rename partitions in a partition table.                                                                      |
         |                                   |    -  **Restore Table Partition**: This permission allows you to export partition information from the file system and save the information to metadata.                     |
         |                                   |    -  **View All Partitions**: This permission allows you to view all partitions in a partition table.                                                                       |
         |                                   |                                                                                                                                                                              |
         |                                   | -  The following permissions can only be granted to users:                                                                                                                   |
         |                                   |                                                                                                                                                                              |
         |                                   |    -  **View Table**: This permission allows you to view the current table.                                                                                                  |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  For details about the view permissions, see :ref:`Table 3 <dli_01_0448__table266011407485>`.

      .. note::

         A view can be created only by using SQL statements. You cannot create a view on the **Create Table** page.

      .. _dli_01_0448__table266011407485:

      .. table:: **Table 3** Parameter description

         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                    |
         +===================================+================================================================================================================================================================================+
         | Authorization Object              | Select **User** or **Project**.                                                                                                                                                |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Username/Project                  | -  If you select **User**, enter the IAM username when adding a user to the database.                                                                                          |
         |                                   |                                                                                                                                                                                |
         |                                   |    .. note::                                                                                                                                                                   |
         |                                   |                                                                                                                                                                                |
         |                                   |       The username is an existing IAM user name and has logged in to the DLI management console.                                                                               |
         |                                   |                                                                                                                                                                                |
         |                                   | -  If you select **Project**, select the project to be authorized in the current region.                                                                                       |
         |                                   |                                                                                                                                                                                |
         |                                   |    .. note::                                                                                                                                                                   |
         |                                   |                                                                                                                                                                                |
         |                                   |       If you select **Project**, you can only view information about the authorized tables and their databases.                                                                |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Non-inheritable Permissions       | Select a permission to grant it to the user, or deselect a permission to revoke it.                                                                                            |
         |                                   |                                                                                                                                                                                |
         |                                   | -  The following permissions are applicable to both user and project authorization:                                                                                            |
         |                                   |                                                                                                                                                                                |
         |                                   |    -  **View Table Information**: This permission allows you to view information about the current table.                                                                      |
         |                                   |    -  **View Table Creation Statement**: This permission allows you to view the statement for creating the current table.                                                      |
         |                                   |    -  **Drop Table**: This permission allows you to delete the current table.                                                                                                  |
         |                                   |    -  **Select Table**: This permission allows you to query data of the current table.                                                                                         |
         |                                   |    -  **Rename Table**: Rename the current table.                                                                                                                              |
         |                                   |    -  **Grant Permission**: The current user or project can grant table permissions to other users or projects.                                                                |
         |                                   |    -  **Revoke Permission**: The current user or project can revoke the table's permissions that other users or projects have but cannot revoke the table owner's permissions. |
         |                                   |    -  **View Other Users' Permissions**: This permission allows you to query other users' permission on the current table.                                                     |
         |                                   |                                                                                                                                                                                |
         |                                   | -  Only applicable to                                                                                                                                                          |
         |                                   |                                                                                                                                                                                |
         |                                   |    -  **View Table**: This permission allows you to view the current table.                                                                                                    |
         +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

Modifying Permissions for an Existing User or Project
-----------------------------------------------------

For a user or project that has some permissions on the database, you can revoke the existing permissions or grant new ones.

.. note::

   If all options under **Set Permission** are gray, you are not allowed to change permissions on this table. You can apply to the administrator, table owner, or other authorized users for granting and revoking table permissions.

#. In the **User Permission Info** list, find the user whose permission needs to be set.

   -  If the user is an IAM user and is not the owner of the table, you can set permissions.
   -  If the user is an administrator or table owner, you can only view permissions.

   In the **Project Permission Info** list, locate the project for which you want to set permissions and click **Set Permission**.

#. In the **Operation** column of the IAM user or project, click **Set Permission**. The **Set Permission** dialog box is displayed.

   -  For details about DLI table user or project permissions, see :ref:`Table 1 <dli_01_0448__table06511040124817>`.
   -  For details about OBS table user or project permissions, see :ref:`Table 2 <dli_01_0448__table36563404488>`.
   -  For details about View table user or project permissions, see :ref:`Table 3 <dli_01_0448__table266011407485>`.

#. Click **OK**.

Revoking All Permissions of a User or Project
---------------------------------------------

Revoke all permissions of a user or a project.

-  In the user list under **User Permission Info**, locate the row where the target IAM user resides and click **Revoke Permission** in the **Operation** column. In the displayed dialog box, click **OK**. In this case, the user has no permissions on the table.

   .. note::

      In the following cases, **Revoke Permission** is gray, indicating that the permission of the user cannot be revoked.

      -  The user is an administrator.
      -  The IAM user is the owner of the table.
      -  The IAM user has only inheritable permissions.

-  In the **Project Permission Info** area, select the project whose permissions need to be revoked and click **Revoke Permission** in the **Operation** column. After you click **OK**, the project does not have any permissions on the table.

   .. note::

      If a project has only inheritable permissions, **Revoke Permission** is gray, indicating that the permissions of the project cannot be revoked.
