:original_name: dli_01_0480.html

.. _dli_01_0480:

Managing Datasource Connection Permissions
==========================================

Scenarios
---------

-  You can isolate datasource connections allocated to different users by setting permissions to ensure data query performance.
-  The administrator and datasource connection owner have all permissions, which cannot be set or modified by other users.

On the **Datasource Authentication** tab page, click **Manage Permissions** in the **Operation** column of the row that contains the authentication to be modified. On the **User Permission Info** page that is displayed, you can grant, set, and revoke permissions of the datasource connection.

Granting Permissions on Datasource Connections
----------------------------------------------

Log in to the DLI management console, choose **Datasource Connections**. Click the **Datasource Authentication** tab, locate the target authentication certificate, click **Permission Management** in the **Operation** column. Click **Grant Permission** in the upper right corner of the page.

.. _dli_01_0480__table15710625151416:

.. table:: **Table 1** Permission granting parameters

   +--------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                        | Description                                                                                                                                                                                   |
   +==================================================+===============================================================================================================================================================================================+
   | Username                                         | Name of the authorized IAM user.                                                                                                                                                              |
   |                                                  |                                                                                                                                                                                               |
   |                                                  | .. note::                                                                                                                                                                                     |
   |                                                  |                                                                                                                                                                                               |
   |                                                  |    The username is the name of an existing IAM user.                                                                                                                                          |
   +--------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Select the permissions to be granted to the user | -  Access: This permission allows you to access the datasource connection.                                                                                                                    |
   |                                                  | -  Update: This permission allows you to update the datasource connection.                                                                                                                    |
   |                                                  | -  Delete: This permission allows you to delete the datasource connection.                                                                                                                    |
   |                                                  | -  Grant Permission: This permission allows you to grant the datasource connection permission to other users.                                                                                 |
   |                                                  | -  Grant Permission: This permission allows you to revoke the datasource connection permission to other users. However, you cannot revoke the permissions of the datasource connection owner. |
   |                                                  | -  View Other User's Permissions: This permission allows you to view the datasource connection permissions of other users.                                                                    |
   +--------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Setting Permissions
-------------------

Click **Set Permission** in the **Operation** column of the sub-user to modify the permission of the user. :ref:`Table 1 <dli_01_0480__table15710625151416>` lists the detailed permission descriptions.

If all options under **Set Permission** are gray, you are not allowed to change permissions on this datasource connection. You can apply for the granting and revoking permissions from administrators, group owners, and other users who have the permission to grant permissions.

Revoking Permissions
--------------------

Click **Revoke Permission** in the **Operation** column of a sub-user to revoke the user's permissions. After this operation, the sub-user does not have any permission of the datasource connection.
