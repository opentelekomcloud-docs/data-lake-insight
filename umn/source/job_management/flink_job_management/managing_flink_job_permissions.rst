:original_name: dli_01_0479.html

.. _dli_01_0479:

Managing Flink Job Permissions
==============================

Scenario
--------

-  You can isolate Flink jobs allocated to different users by setting permissions to ensure data query performance.
-  The administrator and job creator have all permissions, which cannot be set or modified by other users.

Flink Job Permission Operations
-------------------------------

#. On the left of the DLI management console, choose **Job Management** > **Flink Jobs**.

#. Select the job to be configured and choose **More** > **Permissions** in the **Operation** column. The **User Permissions** area displays the list of users who have permissions on the job.

   You can assign queue permissions to new users, modify permissions for users who have some permissions of a queue, and revoke all permissions of a user on a queue.

   -  Assign permissions to a new user.

      A new user does not have permissions on the job.

      a. Click **Grant Permission** on the right of **User Permissions** page. The **Grant Permission** dialog box is displayed.

      b. Specify **Username** and select corresponding permissions.

      c. Click **OK**.

         :ref:`Table 1 <dli_01_0479__table15710625151416>` describes the related parameters.

         .. _dli_01_0479__table15710625151416:

         .. table:: **Table 1** Permission parameters

            +---------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
            | Parameter                             | Description                                                                                                                                               |
            +=======================================+===========================================================================================================================================================+
            | Username                              | Name of the user you want to grant permissions to.                                                                                                        |
            |                                       |                                                                                                                                                           |
            |                                       | .. note::                                                                                                                                                 |
            |                                       |                                                                                                                                                           |
            |                                       |    The username is the name of an existing IAM user. In addition, the user can perform authorization operations only after logging in to the platform.    |
            +---------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
            | Permissions to be granted to the user | -  **Select all**: All permissions are selected.                                                                                                          |
            |                                       | -  **View Job Details**: This permission allows you to view the job details.                                                                              |
            |                                       | -  **Modify Job**: This permission allows you to modify the job.                                                                                          |
            |                                       | -  **Delete Job**: This permission allows you to delete the job.                                                                                          |
            |                                       | -  **Start Job**: This permission allows you to start the job.                                                                                            |
            |                                       | -  **Stop Job**: This permission allows you to stop the job.                                                                                              |
            |                                       | -  **Export Job**: This permission allows you to export the job.                                                                                          |
            |                                       | -  **Grant Permission**: This permission allows you to grant job permissions to other users.                                                              |
            |                                       | -  **Revoke Permission**: This permission allows you to revoke the job permissions that other users have but cannot revoke the job creator's permissions. |
            |                                       | -  **View Other User's Permissions**: This permission allows you to view the job permissions of other users.                                              |
            +---------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  To assign or revoke permissions of a user who has some permissions on the job, perform the following steps:

      a. In the list under **User Permissions** for a job, select the user whose permissions need to be modified and click **Set Permission** in the **Operation** column.

      b. In the displayed **Set Permission** dialog box, modify the permissions of the current user. :ref:`Table 1 <dli_01_0479__table15710625151416>` lists the detailed permission descriptions.

         If all options under **Set Permission** are gray, you are not allowed to change permissions on this job. You can apply to the administrator, job creator, or other authorized users for job permission granting and revoking.

      c. Click **OK**.

   -  To revoke all permissions of a user on a job, perform the following steps:

      In the list under **User Permissions** for a job, locate the user whose permissions need to be revoked, click **Revoke Permission** in the **Operation** column, and click **Yes**. After this operation, the user does not have any permission on the job.

Flink Job Permissions
---------------------

-  **View Job Details**

   -  Tenants and the admin user can view and operate all jobs.
   -  Subusers and users with the read-only permission can only view their own jobs.

      .. note::

         If another user grants any permission other than the job viewing permission to a subuser, the job is displayed in the job list, but the details cannot be viewed by the subuser.

-  **Start Job**

   -  To use a dedicated queue, you must have the permission to submit and start jobs.
   -  To use a shared queue, you only need to have the permission to start jobs.

-  .. _dli_01_0479__li5290184073819:

   **Stop Job**

   -  To use a dedicated queue, you must have the permission to stop jobs and queues.
   -  To use a shared queue, you only need to have the permission to stop jobs.

-  **Delete Job**

   -  If a job can be deleted, you can delete the job if you were granted this permission.
   -  If a job cannot be deleted, the system stops the job before you delete it. For details about how to stop a job, see :ref:`Stop Job <dli_01_0479__li5290184073819>`. In addition, you must have the permission to delete the job.

-  **Create Job**

   -  By default, sub-users cannot create jobs.
   -  To create a job, you must have this permission. Currently, only the admin user has the permission to create jobs. In addition, the user must have the permission of the related package group or package used by the job.

-  **Modify Job**

   When modifying a job, you need to have the permission to update the job and the permission to the package group or package used by the job belongs.
