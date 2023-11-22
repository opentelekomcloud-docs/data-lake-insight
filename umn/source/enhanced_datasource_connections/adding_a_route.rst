:original_name: dli_01_0014.html

.. _dli_01_0014:

Adding a Route
==============

Scenario
--------

A route is configured with the destination, next hop type, and next hop to determine where the network traffic is directed. Routes are classified into system routes and custom routes.

After an enhanced connection is created, the subnet is automatically associated with the default route. You can add custom routes as needed to forward traffic destined for the destination to the specified next hop.

.. note::

   -  When an enhanced connection is created, the associated route table is the one associated with the subnet of the data source.
   -  The route to be added in the **Add Route** dialog box must be one in the route table associated with the subnet of the resource pool.
   -  The subnet of the data source must be different from that used by the resource pool. Otherwise, a network segment conflict occurs.

Procedure
---------

#. Log in to the DLI management console.

#. In the left navigation pane, choose **Datasource Connections**.

#. On the **Enhanced** tab page displayed, locate the row containing the enhanced connection to which a route needs to be added, and add the route.

   -  Method 1:

      a. On the **Enhanced** tab page displayed, locate the enhanced datasource connection to which a route needs to be added and click **Manage Route** in the **Operation** column.
      b. Click **Add Route**.
      c. In the **Add Route** dialog box, enter the route information. For details about the parameters, see :ref:`Table 1 <dli_01_0014__table42440623119>`.
      d. Click **OK**.

   -  Method 2:

      a. On the **Enhanced** tab page displayed, locate the enhanced datasource connection to which a route needs to be added, click **More** in the **Operation** column, and select **Add Route**.
      b. In the **Add Route** dialog box, enter the route information. For details about the parameters, see :ref:`Table 1 <dli_01_0014__table42440623119>`.
      c. Click **OK**.

   .. _dli_01_0014__table42440623119:

   .. table:: **Table 1** Parameters for adding a custom route

      +------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Description                                                                                                                                                                                    |
      +============+================================================================================================================================================================================================+
      | Route Name | Name of a custom route, which is unique in the same enhanced datasource scenario. The name can contain 1 to 64 characters. Only digits, letters, underscores (_), and hyphens (-) are allowed. |
      +------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | IP Address | Custom route CIDR block. The CIDR block of different routes can overlap but cannot be the same.                                                                                                |
      +------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. After adding a route, you can view the route information on the route details page.
