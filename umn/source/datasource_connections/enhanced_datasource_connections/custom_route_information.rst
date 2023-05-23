:original_name: dli_01_0014.html

.. _dli_01_0014:

Custom Route Information
========================

Customizing Route Information
-----------------------------

After an enhanced datasource connection is created and bound to a queue, the system automatically configures route information. You can also add a custom route for the queue to which the enhanced connection is bound.

-  Viewing route information

   On the **Enhanced** tab page, select a connection and click **Manage Route** in the **Operation** column to view the route information of the datasource connection.

-  Adding a route

   On the **Enhanced** tab page, select a connection and choose **More** > **Add Route** in the **Operation** column, or click **Add Route** on the **Details** page of the connection to add a custom route. In the displayed dialog box, enter the route name and route CIDR block. For details about the parameters, see :ref:`Table 1 <dli_01_0014__table42440623119>`.

   .. _dli_01_0014__table42440623119:

   .. table:: **Table 1** Parameters for adding a custom route

      +------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Description                                                                                                                                                                          |
      +============+======================================================================================================================================================================================+
      | Route Name | Name of a custom route, which is unique in the same enhanced datasource scenario. The name contains 1 to 64 characters, including digits, letters, underscores (_), and hyphens (-). |
      +------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | IP Address | Custom route CIDR block. The CIDR block of different routes can overlap but cannot be the same.                                                                                      |
      +------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Deleting a route

   On the **Enhanced** page, select a connection and choose **More** > **Delete Route** in the **Operation** column, or click **Delete Route** on the **Details** page of the connection to delete a custom route.
