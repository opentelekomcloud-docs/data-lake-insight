:original_name: dli_01_0014.html

.. _dli_01_0014:

Adding a Route for an Enhanced Datasource Connection
====================================================

Scenario
--------

Enhanced datasource connections establish peering connections between DLI elastic resource pools and data sources to interconnect two VPC networks. If we liken these peering connections to exclusive bridges linking elastic resource pools and data sources, then routing serves as directional signage guiding the flow of network traffic.

Routing rules determine the direction of network traffic by configuring information such as the destination address, next hop type, and next hop address. Routes are classified into system routes and custom routes.

After an enhanced datasource connection is created, the subnet is automatically associated with the system default route. Besides the system default route, you can add custom routing rules as needed to forward traffic destined for the target address to the specified next-hop address.

.. note::

   -  When an enhanced connection is created, the associated route table is the one associated with the subnet of the data source.
   -  The route to be added in the **Add Route** dialog box must be one in the route table associated with the subnet of the resource pool.
   -  The data source subnet and the elastic resource pool subnet must be different subnets to avoid network segment conflicts.

Procedure
---------

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Datasource Connections**.

#. On the displayed **Enhanced** tab, locate the enhanced datasource connection where you want to add a route and then add a route.

   -  Method 1:

      a. On the **Enhanced** tab, locate the enhanced datasource connection where you want to add a route and click **Manage Route** in its **Operation** column.
      b. On the displayed page, click **Add Route**.
      c. In the **Add Route** dialog box, set the parameters based on :ref:`Table 1 <dli_01_0014__table42440623119>`.
      d. Click **OK**.

   -  Method 2:

      a. On the **Enhanced** tab, locate the enhanced datasource connection where you want to add a route, click **More** in its **Operation** column, and select **Add Route**.
      b. In the **Add Route** dialog box, set the parameters based on :ref:`Table 1 <dli_01_0014__table42440623119>`.
      c. Click **OK**.

   .. _dli_01_0014__table42440623119:

   .. table:: **Table 1** Parameters for adding a custom route

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                              |
      +===================================+==========================================================================================================================================================================================================+
      | Route Name                        | Name of a custom route, which is unique in the same enhanced datasource connection. The name can contain up to 64 characters. Only digits, letters, underscores (_), and hyphens (-) are allowed.        |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | IP Address                        | Custom route CIDR block. The CIDR blocks of different routes can overlap but cannot be identical.                                                                                                        |
      |                                   |                                                                                                                                                                                                          |
      |                                   | Do not add the **100.125.**\ *xx.xx* or **100.64.**\ *xx.xx* CIDR blocks to avoid conflicts with the internal CIDR blocks of services like SWR, which can cause enhanced datasource connections to fail. |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. After adding a route, you can view the route information on the route details page.
