:original_name: dli_01_0477.html

.. _dli_01_0477:

Configuring DLI Package Permissions
===================================

By configuring permissions, you can grant different package groups or packages to various users, ensuring that job efficiency remains unaffected and job performance is maintained.

-  Administrators and package group owners have all permissions for the package group. No permission settings are required, and other users cannot modify their package group permissions.

-  Administrators and package owners have all permissions for the package. No permission settings are required, and other users cannot modify their package permissions.

-  Package groups are used to manage packages with consistent behavior, so they support granting related permissions to package groups, but do not support granting individual permissions to packages within a package group.

-  When an administrator assigns package group or package permissions to a new user, the administrator's user group must have Tenant Guest permissions.

Configuring Permissions on Package Groups or Packages
-----------------------------------------------------

#. On the **Data Management** > **Package Management** page, locate the package group or package whose permissions you want to grant and click **Manage Permission** in the **Operation** column.
#. On the displayed **User Permissions** page, click **Grant Permission** in the upper right corner of the page. In the dialog box that appears, enter a username in **Username**, select required permissions, and click **OK**.

   .. note::

      -  If you select a group when creating a package, you can manage permissions of the corresponding program package group.
      -  If you select **No grouping** when creating a package, you can manage permissions of the corresponding package.

   -  Granting permissions on package groups

      .. _dli_01_0477__table197423318295:

      .. table:: **Table 1** Permission parameters

         +--------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                                        | Description                                                                                                                                                 |
         +==================================================+=============================================================================================================================================================+
         | Username                                         | Name of the authorized IAM user.                                                                                                                            |
         |                                                  |                                                                                                                                                             |
         |                                                  | .. note::                                                                                                                                                   |
         |                                                  |                                                                                                                                                             |
         |                                                  |    The username is the name of an existing IAM user.                                                                                                        |
         +--------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Select the permissions to be granted to the user | -  **Use Group**: This permission allows you to use the package of this group.                                                                              |
         |                                                  | -  **Update Group**: This permission allows you to update the packages in the group, including creating a package in the group.                             |
         |                                                  | -  **Query Group**: This permission allows you to query the details of a package in a group.                                                                |
         |                                                  | -  **Delete Group**: This permission allows you to delete the package of the group.                                                                         |
         |                                                  | -  **Grant Permission**: This permission allows you to grant group permissions to other users.                                                              |
         |                                                  | -  **Revoke Permission**: This permission allows you to revoke the group permissions that other users have but cannot revoke the group owner's permissions. |
         |                                                  | -  **View Other User's Permissions**: This permission allows you to view the group permissions of other users.                                              |
         +--------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  Granting permissions on packages

      .. _dli_01_0477__table1382533416195:

      .. table:: **Table 2** Permission parameters

         +--------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                                        | Description                                                                                                                                                     |
         +==================================================+=================================================================================================================================================================+
         | Username                                         | Name of the authorized IAM user.                                                                                                                                |
         |                                                  |                                                                                                                                                                 |
         |                                                  | .. note::                                                                                                                                                       |
         |                                                  |                                                                                                                                                                 |
         |                                                  |    The username is the name of an existing IAM user.                                                                                                            |
         +--------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Select the permissions to be granted to the user | -  **Use Package**: This permission allows you to use the package.                                                                                              |
         |                                                  | -  **Update Package**: This permission allows you to update the package.                                                                                        |
         |                                                  | -  **Query Package**: This permission allows you to query the package.                                                                                          |
         |                                                  | -  **Delete Package**: This permission allows you to delete the package.                                                                                        |
         |                                                  | -  **Grant Permission**: This permission allows you to grant package permissions to other users.                                                                |
         |                                                  | -  **Revoke Permission**: This permission allows you to revoke the package permissions that other users have but cannot revoke the package owner's permissions. |
         |                                                  | -  **View Other User's Permissions**: This permission allows you to view the package permissions of other users.                                                |
         +--------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+

Modifying Permissions on Package Groups or Packages
---------------------------------------------------

#. On the **Data Management** > **Package Management** page, locate the desired package group or package and click **Manage Permission** in the **Operation** column.

#. On the displayed **User Permissions** page, click **Set Permission** in the **Operation** column of the IAM user for whom you want to modify the permissions.

   :ref:`Table 1 <dli_01_0477__table197423318295>` and :ref:`Table 2 <dli_01_0477__table1382533416195>` list the detailed permission descriptions.

   .. note::

      -  If you set **Group** to **Use existing** or **Use new** when creating a package, you will modify the permissions on the group you selected.
      -  If you set **Group** to **Do not use** when creating a package, you will modify the permissions on the package.

   If **Set Permission** on the **User Permissions** page is grayed out, you do not have the permission to modify the permissions on the package group or package.

   You can request the **Grant Permission** and **Revoke Permission** permissions on package groups or packages from users who have authorization privileges, such as administrators or group owners.

Revoking Permissions on Package Groups or Packages
--------------------------------------------------

DLI allows you to revoke permissions on package groups or packages with just one click.

#. On the **Data Management** > **Package Management** page, locate the desired package group or package and click **Manage Permission** in the **Operation** column.

#. On the displayed **User Permissions** page, click **Revoke Permission** in the **Operation** column of the IAM user for whom you want to revoke the permissions.

   Once the permissions are revoked, the IAM user does not have any permissions on the package group or package.

   .. note::

      -  If you set **Group** to **Use existing** or **Use new** when creating a package, you will revoke the permissions on the group you selected.
      -  If you set **Group** to **Do not use** when creating a package, you will revoke the permissions on the package.
