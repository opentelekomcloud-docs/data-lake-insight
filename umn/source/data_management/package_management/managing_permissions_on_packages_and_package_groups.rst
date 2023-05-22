:original_name: dli_01_0477.html

.. _dli_01_0477:

Managing Permissions on Packages and Package Groups
===================================================

Scenarios
---------

-  You can isolate package groups or packages allocated to different users by setting permissions to ensure data query performance.
-  The administrator and the owner of a package group or package have all permissions. You do not need to set permissions and the permissions cannot be modified by other users.
-  When you set permissions on a package group or a package to a new user, the user group to which the user belongs must have the Tenant Guest permission. For details about the Tenant Guest permission and how to apply for the permission, see .

On the **Package Management** page, click **Manage Permissions** in the **Operation** column of the target package. On the displayed **User Permission Info** page, you can grant permissions for the package group or package, set and revoke user permissions.

.. note::

   -  If you select a group when creating a package, you can manage permissions of the corresponding program package group.
   -  If you select **No grouping** when creating a package, you can manage permissions of the corresponding package.

Granting Permissions on Package Groups/Packages
-----------------------------------------------

Click **Grant Permission** in the upper right corner of the page.

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

Setting Permissions on Package Groups and Packages
--------------------------------------------------

Click **Set Permission** in the **Operation** column of the sub-user to modify the permission of the user. :ref:`Table 1 <dli_01_0477__table197423318295>` and :ref:`Table 2 <dli_01_0477__table1382533416195>` list the detailed permission descriptions.

If the **Set Permission** button is gray, you do not have the permission to modify the package group or package. You can apply to the administrator, group owner, or other users who have the permissions on granting and revoking permissions of package groups or packages.

Revoking Permissions on Package Groups and Packages
---------------------------------------------------

Click **Revoke Permission** in the **Operation** column of a sub-user to revoke the user's permissions. After the operation, the sub-user does not have any permission on the package group or package.

Permissions Description
-----------------------

-  Package group permissions

   Querying permissions. A group owner can view the created package group and all packages in the group, and can also view package groups on which he or she has all permissions.

   A package group is a unit. If you select a group when creating a package, you can grant only the permissions of the package group to other users.

-  Package permissions

   Querying permissions. A package owner can view the created packages, and can also view packages on which he or she has all permissions.
