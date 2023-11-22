:original_name: dli_01_0533.html

.. _dli_01_0533:

Permission Management for Global Variables
==========================================

Scenario
--------

-  You can grant permissions on a global variable to users.
-  The administrator and the global variable owner have all permissions. You do not need to set permissions for them, and their global variable permissions cannot be modified by other users.
-  When setting global variables for a new user, ensure that the user group the user belongs to has the **Tenant Guest** permission. For details about the Tenant Guest permission and how to apply for the permission, see .

Granting Permissions on a Global Variable to a User
---------------------------------------------------

#. Choose **Global Configuration** > **Global Variables**, locate the row containing the desired global variable, and click **Set Permission** in the **Operation** column. On the **User Permissions** page displayed, you can grant permissions for the global variable, set and revoke user permissions.

#. Click **Grant Permission** in the upper right corner of the page.

   .. _dli_01_0533__table1382533416195:

   .. table:: **Table 1** Global variable parameters

      +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                             | Description                                                                                                                                                                     |
      +=======================================+=================================================================================================================================================================================+
      | Username                              | Name of the IAM user to whom permissions on the datasource connection are to be granted.                                                                                        |
      |                                       |                                                                                                                                                                                 |
      |                                       | .. note::                                                                                                                                                                       |
      |                                       |                                                                                                                                                                                 |
      |                                       |    The username is the name of an existing IAM user.                                                                                                                            |
      +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Permissions to be granted to the user | -  **Update**: This permission allows you to update the global variable.                                                                                                        |
      |                                       | -  **Delete**: This permission allows you to delete the global variable.                                                                                                        |
      |                                       | -  **Grant Permission**: This permission allows you to grant permissions of the global variable to other users.                                                                 |
      |                                       | -  **Revoke Permission**: This permission allows you to revoke the global variable permissions that other users have but cannot revoke the global variable owner's permissions. |
      |                                       | -  **View Other User's Permissions**: This permission allows you to view the global variable permissions of other users.                                                        |
      +---------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Granting Global Variable Permissions
------------------------------------

Click **Set Permission** in the **Operation** column of the sub-user to modify their permissions. :ref:`Table 1 <dli_01_0533__table1382533416195>` lists the permission parameters.

If all permission options are grayed out, you are not allowed to change permissions on this global variable. You can apply to the administrator, group owner, or other authorized users for required permissions on the global variable.

Revoking Global Variable Permissions
------------------------------------

Click **Revoke Permission** in the **Operation** column of a sub-user to revoke their permissions. After this operation, the sub-user does not have any permission on the global variable.
