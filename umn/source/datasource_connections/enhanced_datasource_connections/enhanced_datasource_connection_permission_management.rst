:original_name: dli_01_0018.html

.. _dli_01_0018:

Enhanced Datasource Connection Permission Management
====================================================

Permission Management
---------------------

You can grant or revoke permissions for enhanced datasource connections of other projects.

-  Authorization

   #. Log in to the DLI management console, choose **Datasource Connections** and select the **Enhanced** tab, select a datasource connection, and choose **More** > **Manage Permission** in the **Operation** column. In the displayed **Permissions** dialog box, select **Grant Permission**, enter the project ID, and click **OK**.
   #. After a project is authorized, you can log in to the system as a user of the authorized project or switch to the corresponding project. In the **Enhanced** tab, you can view the authorized datasource connection and bind the created queue to the datasource connection. Cross-project datasource connections and routes can be created.

   .. note::

      -  If the authorized projects belong to different users in the same region, you can use the user account of the authorized projects to log in.
      -  If the authorized projects belong to the same user in the same region, you can use the current account to switch to the corresponding project.

   For example, if project B needs to access the data source of project A, perform the following operations:

   -  For Project A:

      #. Log in to DLI using the account of project A.
      #. Create an enhanced datasource connection **ds** in DLI based on the VPC information of the corresponding data source.
      #. Grant project B the permission to access the enhanced datasource connection **ds**.

   -  For Project B:

      #. Log in to DLI using the account of project B.
      #. Bind the enhanced datasource connection **ds** to a queue.
      #. (Optional) Set host information and create a route.

   After creating a VPC peering connection and route between the enhanced datasource connection of project A and the queue of project B, you can create a job in the queue of project B to access the data source of project A.

-  Revoke: In the **Manage Permissions** dialog box, select **Revoke Permission** and select the ID of the project whose permissions need to be revoked.
