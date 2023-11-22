:original_name: dli_01_0480.html

.. _dli_01_0480:

Datasource Authentication Permission Management
===============================================

Scenario
--------

Grant permissions on a datasource authentication to users so multiple user jobs can use the datasource authentication without affecting each other.

Notes
-----

-  The administrator and the datasource authentication owner have all permissions. You do not need to set permissions for them, and their datasource authentication permissions cannot be modified by other users.
-  When setting datasource authentication permissions for a new user, ensure that the user group to which the user belongs has the **Tenant Guest** permission.

Granting Permissions on Datasource Connections
----------------------------------------------

#. Log in to the DLI management console.

#. Choose **Datasource Connections**. On the page displayed, click **Datasource Authentication**.

#. Locate the row containing the datasource authentication to be authorized and click **Manage Permission** in the **Operation** column. The **User Permissions** page is displayed.

#. Click **Grant Permission** in the upper right corner of the page. On the **Grant Permission** dialog box displayed, grant permissions on this datasource authentication to other users.

   .. _dli_01_0480__table15710625151416:

   .. table:: **Table 1** Permission granting parameters

      +--------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                                        | Description                                                                                                                                                                                   |
      +==================================================+===============================================================================================================================================================================================+
      | Username                                         | Name of the IAM user to whom permissions on the datasource connection are to be granted.                                                                                                      |
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

Modifying the Permissions of Current User
-----------------------------------------

#. Log in to the DLI management console.
#. Choose **Datasource Connections**. On the page displayed, click **Datasource Authentication**.
#. Locate the row containing the datasource authentication to be authorized and click **Manage Permission** in the **Operation** column. The **User Permissions** page is displayed.
#. Click **Set Permission** in the **Operation** column to modify the permissions of the current user. :ref:`Table 1 <dli_01_0480__table15710625151416>` lists the detailed permission descriptions.

   .. note::

      -  If all options under **Set Permission** are gray, you are not allowed to change permissions on this datasource connection. You can apply to the administrator, group owner, or other users who have the permission to grant permissions for the permissions to grant and revoke the datasource authentication permissions.
      -  To revoke all permissions of the current user, click **Revoke Permission** in the **Operation** column. The IAM user will no longer have any permission on the datasource authentication.
