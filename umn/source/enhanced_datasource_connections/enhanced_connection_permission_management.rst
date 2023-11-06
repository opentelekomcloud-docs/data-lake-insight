:original_name: dli_01_0018.html

.. _dli_01_0018:

Enhanced Connection Permission Management
=========================================

Scenario
--------

Enhanced connections support user authorization by project. After authorization, users in the project have the permission to perform operations on the enhanced connection, including viewing the enhanced connection, binding a created resource pool to the enhanced connection, and creating custom routes. In this way, the enhanced connection can be used across projects. Grant and revoke permissions to and from a user for an enhanced connection.

.. note::

   -  If the authorized projects belong to different users in the same region, you can use the user account of the authorized projects to log in.
   -  If the authorized projects belong to the same user in the same region, you can use the current account to switch to the corresponding project.

Use Cases
---------

Project B needs to access the data source of project A. The operations are as follows:

-  For Project A:

   #. Log in to DLI using the account of project A.
   #. Create an enhanced datasource connection **ds** in DLI based on the VPC information of the corresponding data source.
   #. Grant project B the permission to access the enhanced datasource connection **ds**.

-  For Project B:

   #. Log in to DLI using the account of project B.
   #. Bind the enhanced datasource connection **ds** to a queue.
   #. (Optional) Set host information and create a route.

After creating a VPC peering connection and route between the enhanced datasource connection of project A and the queue of project B, you can create a job in the queue of project B to access the data source of project A.

Procedure
---------

#. Log in to the DLI management console.
#. In the left navigation pane, choose **Datasource Connections**.
#. On the **Enhanced** tab page displayed, locate the desired enhanced connection, click **More** in the **Operation** column, and select **Manage Permission**.

   -  **Granting permission**

      a. In the **Permissions** dialog box displayed, select **Grant Permission** for **Set Permission**.
      b. Enter the project ID.
      c. Click **OK** to grant the resource pool operation permission to the project.

   -  **Revoking permission**

      a. In the **Permissions** dialog box displayed, select **Revoke Permission** for **Set Permission**.
      b. Select a project ID.
      c. Click **OK** to revoke the resource pool operation permission from the specified project.
