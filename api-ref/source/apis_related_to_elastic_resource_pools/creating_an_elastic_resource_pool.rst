:original_name: dli_02_0326.html

.. _dli_02_0326:

Creating an Elastic Resource Pool
=================================

Function
--------

This API is used to create elastic resource pools.

URI
---

-  URI format

   POST /v3/{project_id}/elastic-resource-pools

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                          |
      +============+===========+========+======================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For how to obtain the project ID, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------+

Request Parameters
------------------

.. table:: **Table 2** Request parameters

   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory       | Type             | Description                                                                                                                                                                                                      |
   +============================+=================+==================+==================================================================================================================================================================================================================+
   | elastic_resource_pool_name | Yes             | String           | Name of a new elastic resource pool. Only digits, letters, and underscores (_) are allowed, but the value cannot contain only digits or start with an underscore (_). The value can contain 1 to 128 characters. |
   |                            |                 |                  |                                                                                                                                                                                                                  |
   |                            |                 |                  | .. note::                                                                                                                                                                                                        |
   |                            |                 |                  |                                                                                                                                                                                                                  |
   |                            |                 |                  |    If the name contains uppercase letters, the system automatically converts them to lowercase letters.                                                                                                          |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | description                | No              | String           | Description. The value can contain a maximum of 256 characters.                                                                                                                                                  |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cidr_in_vpc                | No              | String           | VPC CIDR associated with the virtual cluster. If it is not specified, the default value **172.16.0.0/12** is used.                                                                                               |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_cu                     | Yes             | Integer          | Maximum number of CUs. The value of this parameter must be greater than or equal to the sum of the maximum CUs allowed for any queue within the resource pool, and greater than the **min_cu** value.            |
   |                            |                 |                  |                                                                                                                                                                                                                  |
   |                            |                 |                  | -  Standard edition: The minimum value is 64 CUs.                                                                                                                                                                |
   |                            |                 |                  | -  Basic edition: The minimum value is 16 CUs, and the maximum value is 64 CUs.                                                                                                                                  |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_cu                     | Yes             | Integer          | Minimum number of CUs. The value of this parameter must be greater than or equal to the sum of the minimum CUs allowed for each queue in the resource pool. The minimum value is **64**.                         |
   |                            |                 |                  |                                                                                                                                                                                                                  |
   |                            |                 |                  | -  Standard edition: The minimum value is 64 CUs.                                                                                                                                                                |
   |                            |                 |                  | -  Basic edition: The minimum value is 16 CUs, and the maximum value is 64 CUs.                                                                                                                                  |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | charging_mode              | No              | Integer          | Billing mode. The default value is **1**, which indicates the pay-per-use billing mode.                                                                                                                          |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enterprise_project_id      | No              | String           | Enterprise ID. If this parameter is left blank, the default value **0** is used.                                                                                                                                 |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tags                       | No              | Array of objects | Queue tags for identifying cloud resources. A tag consists of a key and a value. For details, see :ref:`Table 3 <dli_02_0326__table9391124139>`.                                                                 |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | label                      | No              | map              | Attribute field of the elastic resource pool.                                                                                                                                                                    |
   |                            |                 |                  |                                                                                                                                                                                                                  |
   |                            |                 |                  | To purchase the basic version, set this parameter to **{"spec":"basic"}**.                                                                                                                                       |
   |                            |                 |                  |                                                                                                                                                                                                                  |
   |                            |                 |                  | If not set, the default is the standard version.                                                                                                                                                                 |
   +----------------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0326__table9391124139:

.. table:: **Table 3** tags parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                     |
   +=================+=================+=================+=================================================================================================================================================================================================================+
   | key             | Yes             | String          | Tag key                                                                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 | .. note::                                                                                                                                                                                                       |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 |    A tag key can contain a maximum of 128 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed, but the value cannot start or end with a space or start with **\_sys\_**. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value           | Yes             | String          | Tag value                                                                                                                                                                                                       |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 | .. note::                                                                                                                                                                                                       |
   |                 |                 |                 |                                                                                                                                                                                                                 |
   |                 |                 |                 |    A tag value can contain a maximum of 255 characters. Only letters, numbers, spaces, and special characters ``(_.:+-@)`` are allowed.                                                                         |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response Parameters
-------------------

.. table:: **Table 4** Response parameters

   +----------------------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory | Type    | Description                                                                                                       |
   +============================+===========+=========+===================================================================================================================+
   | is_success                 | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +----------------------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message                    | No        | String  | Message content, for example, **Success to get tsdb list**.                                                       |
   +----------------------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | elastic_resource_pool_name | No        | String  | Elastic resource pool name, for example, **elastic_pool_0623_02"**.                                               |
   +----------------------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Create an elastic resource pool with maximum CUs of 684 and minimum CUs of 684.

.. code-block::

   {
     "elastic_resource_pool_name" : "elastic_pool_0623_02",
     "description" : "test",
     "cidr_in_vpc" : "172.16.0.0/14",
     "charging_mode" : "1",
     "max_cu" : 684,
     "min_cu" : 684
   }

Example Response
----------------

.. code-block::

   {
     "is_success" : true,
     "message" : "Success to get tsdb list",
     "elastic_resource_pool_name" : "elastic_pool_0623_02"
   }

Status Codes
------------

+-------------+--------------------------------------------------------------------------------+
| Status Code | Description                                                                    |
+=============+================================================================================+
| 200         | OK                                                                             |
+-------------+--------------------------------------------------------------------------------+
| 400         | Incorrect parameters. For example, creating an existing elastic resource pool. |
+-------------+--------------------------------------------------------------------------------+
| 403         | Forbidden                                                                      |
+-------------+--------------------------------------------------------------------------------+

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
