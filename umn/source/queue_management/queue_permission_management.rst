:original_name: dli_01_0015.html

.. _dli_01_0015:

Queue Permission Management
===========================

Scenario
--------

-  You can isolate queues allocated to different users by setting permissions to ensure data query performance.
-  The administrator and queue owner have all permissions, which cannot be set or modified by other users.

Operations
----------

#. On the top menu bar of the DLI management console, click **Resources** > **Queue Management**.

#. Select the queue to be configured and choose **Manage Permissions** in the **Operation** column. The **User Permission Info** area displays the list of users who have permissions on the queue.

   You can assign queue permissions to new users, modify permissions for users who have some permissions of a queue, and revoke all permissions of a user on a queue.

   -  Assign permissions to a new user.

      A new user does not have permissions on the queue.

      a. Click **Set Permission** on the right of **User Permissions** page. The **Set Permission** dialog box is displayed.

      b. Specify **Username** and select corresponding permissions.

      c. Click **OK**.

         :ref:`Table 1 <dli_01_0015__table15710625151416>` describes the related parameters.

         .. _dli_01_0015__table15710625151416:

         .. table:: **Table 1** Parameter description

            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | Parameter                         | Description                                                                                                                                                 |
            +===================================+=============================================================================================================================================================+
            | Username                          | Name of the authorized user.                                                                                                                                |
            |                                   |                                                                                                                                                             |
            |                                   | .. note::                                                                                                                                                   |
            |                                   |                                                                                                                                                             |
            |                                   |    The username is an existing IAM user name and has logged in to the DLI management console.                                                               |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | Permission Settings               | -  **Delete Queues**: This permission allows you to delete the queue.                                                                                       |
            |                                   | -  **Submit Jobs**: This permission allows you to submit jobs using this queue.                                                                             |
            |                                   | -  **Terminate Jobs**: This permission allows you to terminate jobs submitted using this queue.                                                             |
            |                                   | -  **Grant Permission**: This permission allows you to grant queue permissions to other users.                                                              |
            |                                   | -  **Revoke Permission**: This permission allows you to revoke the queue permissions that other users have but cannot revoke the queue owner's permissions. |
            |                                   | -  **View Other User's Permissions**: This permission allows you to view the queue permissions of other users.                                              |
            |                                   | -  **Restart Queues**: This permission allows you to restart queues.                                                                                        |
            |                                   | -  **Modify Queue Specifications**: This permission allows you to modify queue specifications.                                                              |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  To assign or revoke permissions of a user who has some permissions on the queue, perform the following steps:

      a. In the list under **User Permission Info** for a queue, select the user whose permissions need to be modified and click **Set Permission** in the **Operation** column.

      b. In the displayed **Set Permission** dialog box, modify the permissions of the current user. :ref:`Table 1 <dli_01_0015__table15710625151416>` lists the detailed permission descriptions.

         If all options under **Set Permission** are gray, you are not allowed to change permissions on this queue. You can apply to the administrator, queue owner, or other authorized users for queue permission granting and revoking.

      c. Click **OK**.

   -  To revoke all permission of a user on the queue, perform the following steps:

      In the user list under **Permission Info**, select the user whose permission needs to be revoked and click **Revoke Permission** under **Operation**. In the **Revoke Permission** dialog box, click **OK**. All permissions on this queue are revoked.
