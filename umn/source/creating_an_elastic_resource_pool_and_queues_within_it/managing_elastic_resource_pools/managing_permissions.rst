:original_name: dli_01_0526.html

.. _dli_01_0526:

Managing Permissions
====================

Administrators can assign permissions of different operation scopes to users for each elastic resource pool.

Precautions
-----------

-  The administrator and elastic resource pool owner have all permissions, which cannot be set or modified by other users.

Procedure
---------

#. In the navigation pane on the left of the DLI console, choose **Resources** > **Resource Pool**.

#. Select the desired elastic resource pool and choose **More** > **Permissions** in the **Operation** column. The **User Permissions** area displays the list of users who have permissions of elastic resource pools.

   You can assign permissions to new users, modify permissions for users who already have some permissions of elastic resource pools, and revoke all permissions of a user on a pool.

   -  Assign permissions to a new user.

      A new user does not have permissions on the elastic resource pool.

      a. Click **Set Permission** in the **Operations** column on **User Permissions** page. The **Set Permission** dialog box is displayed.

      b. Set **Username** to the name of the desired IAM user, and select the required permissions for the user.

      c. Click **OK** to.

         :ref:`Table 1 <dli_01_0526__table15710625151416>` describes the related parameters.

         .. _dli_01_0526__table15710625151416:

         .. table:: **Table 1** Parameters

            +--------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+
            | Parameter                                        | Description                                                                                                       |
            +==================================================+===================================================================================================================+
            | Username                                         | Name of the user you want to grant permissions to                                                                 |
            |                                                  |                                                                                                                   |
            |                                                  | .. note::                                                                                                         |
            |                                                  |                                                                                                                   |
            |                                                  |    The username must be an existing IAM username and has logged in to the DLI management console.                 |
            +--------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+
            | Select the permissions to be granted to the user | -  Update: Update the description of an elastic resource pool.                                                    |
            |                                                  | -  Resources: Add queues, delete queues, and configure scaling policies for queues in an elastic resource pool.   |
            |                                                  | -  Delete: Delete the elastic resource pool.                                                                      |
            |                                                  | -  Grant Permission: Grant the elastic resource pool permissions to other users.                                  |
            |                                                  | -  Revoke Permission: Revoke the permissions that other users have but you cannot revoke the owner's permissions. |
            |                                                  | -  View Other User's Permissions: View the elastic resource pool permissions of other users.                      |
            +--------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+

   -  To assign or revoke permissions of a user who has some permissions on the elastic resource pool, perform the following steps:

      a. In the list under **User Permissions**, select the user whose permissions need to be modified and click **Set Permission** in the **Operation** column.

      b. In the displayed **Set Permission** dialog box, modify the permissions of the user. :ref:`Table 1 <dli_01_0526__table15710625151416>` lists the detailed permission descriptions.

         If **Set Permission** is gray, you are not allowed to change permissions on this elastic resource pool. You can apply to the administrator, elastic resource pool owner, or other authorized users for granting and revoking permissions.

      c. Click **OK**.

   -  To revoke all permissions of a user on an elastic resource pool, perform the following steps:

      In the list under **User Permissions**, select the desired user whose permissions and click **Set Permission** in the **Operation** column. Click **Yes** in the **Revoke Permission** dialog box.
