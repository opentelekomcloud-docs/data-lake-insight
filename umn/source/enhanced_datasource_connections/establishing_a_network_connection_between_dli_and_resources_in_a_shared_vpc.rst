:original_name: dli_01_0624.html

.. _dli_01_0624:

Establishing a Network Connection Between DLI and Resources in a Shared VPC
===========================================================================

VPC Sharing Overview
--------------------

VPC sharing allows sharing VPC resources created in one account with other accounts using Resource Access Manager (RAM). For example, account A can share its VPC and subnets with account B. After accepting the share, account B can view the shared VPC and subnets and use them to create resources.

DLI Use Cases
-------------

An enterprise IT management account creates a VPC and subnets and shares them with other service accounts to facilitate centralized configuration of VPC security policies and orderly resource management.

Service accounts use the shared VPC and subnets to create resources and want to use DLI to submit jobs and access resources in the shared VPC. To do this, they need to establish a network connection between DLI and the resources in the shared VPC.

For example, account A is the enterprise IT management account and the owner of VPC resources. It creates the VPC and subnets and shares them with service account B.

Account B is a service account that uses the shared VPC and subnets to create resources and uses DLI to access them.

Prerequisites
-------------

Account A, as the resource owner, has created a VPC and subnets and designated account B as the principal.


Establishing a Network Connection Between DLI and Resources in a Shared VPC
---------------------------------------------------------------------------

#. .. _dli_01_0624__li18199101878:

   Account A creates an enhanced datasource connection.

   a. Log in to the DLI management console using account A.

   b. In the navigation pane on the left, choose **Datasource Connections**.

   c. On the displayed **Enhanced** tab, click **Create**.

      Set parameters based on :ref:`Table 1 <dli_01_0624__table9495521165111>`.

      .. _dli_01_0624__table9495521165111:

      .. table:: **Table 1** Parameters for creating an enhanced datasource connection

         +------------------+----------------------------------------------------------------------------------+
         | Parameter        | Description                                                                      |
         +==================+==================================================================================+
         | Connection Name  | Name of the datasource connection to be created                                  |
         +------------------+----------------------------------------------------------------------------------+
         | Resource Pool    | You do not need to set this parameter in this scenario.                          |
         +------------------+----------------------------------------------------------------------------------+
         | VPC              | VPC shared by account A to account B                                             |
         +------------------+----------------------------------------------------------------------------------+
         | Subnet           | Subnet shared by account A to account B                                          |
         +------------------+----------------------------------------------------------------------------------+
         | Host Information | You do not need to set this parameter in this scenario.                          |
         +------------------+----------------------------------------------------------------------------------+
         | Tags             | Tags used to identify cloud resources. A tag includes the tag key and tag value. |
         +------------------+----------------------------------------------------------------------------------+

   d. Click **OK**.

#. Account A grants account B access to the enhanced datasource connection created in :ref:`1 <dli_01_0624__li18199101878>`.

   a. In the enhanced datasource connection list, locate the row containing the newly created one, click **More** in the **Operation** column, and select **Manage Permission** from the drop-down list.
   b. In the displayed **Permissions** dialog box, select **Grant Permission** for **Set Permission**, enter the ID of the project account B belongs to in **Project ID**, and click **OK**.

#. Account B binds a DLI elastic resource pool to the shared enhanced datasource connection.

   a. Log in to the DLI management console using account B.

   b. In the navigation pane on the left, choose **Datasource Connections**.

   c. On the displayed **Enhanced** tab, locate the row containing the enhanced datasource connection shared by account A, click **More** in the **Operation** column, and select **Bind Resource Pool** from the drop-down list.

   d. In the displayed **Bind Resource Pool** dialog box, select the created elastic resource pool for **Resource Pool** and click **OK**.

      If there is no elastic resource pool available, create one by referring to :ref:`Creating an Elastic Resource Pool <dli_01_0505>`.

#. Account B tests the network connectivity between the elastic resource pool and resources in the VPC.

   .. note::

      If there are resources in the shared VPC, ensure that the security group the resources belong to has allowed access to the elastic resource pool's CIDR block.

   a. Obtain the private IP address and port number of the data source in the shared VPC.

      Take the RDS data source as an example. On the **Instances** page, click the target DB instance. On the displayed page, locate the **Connection Information** pane and view the private IP address. In the **Connection Information** pane, locate the **Database Port** to view the port number of the RDS DB instance.

   b. In the navigation pane of the DLI management console, choose **Resources** > **Queue Management**.

   c. Locate the queue under the elastic resource pool bound with the enhanced datasource connection, click **More** in the **Operation** column, and select **Test Address Connectivity**.

   d. Enter the data source connection address and port number to test the network connectivity.

      If the address is reachable, it means that account B has established a network connection between the DLI resource and the resources in the shared VPC. Account B can then submit jobs to the elastic resource pool's queue and access the resources in the shared VPC.
