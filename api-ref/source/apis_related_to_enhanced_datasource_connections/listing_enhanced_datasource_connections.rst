:original_name: dli_02_0190.html

.. _dli_02_0190:

Listing Enhanced Datasource Connections
=======================================

Function
--------

This API is used to list the created enhanced datasource connections.

URI
---

-  URI format

   GET /v2.0/{project_id}/datasource/enhanced-connections

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                           |
      +=================+=================+=================+=======================================================================================================================================================+
      | limit           | No              | String          | The maximum number of connections to be queried. The default value is **100**. If **limit** is set to **0**, all datasource connections are returned. |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
      | offset          | No              | String          | The offset of the query result. The default value is **0**. Note that the connections are sorted by creation time.                                    |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
      | status          | No              | String          | Connection status. The options are as follows:                                                                                                        |
      |                 |                 |                 |                                                                                                                                                       |
      |                 |                 |                 | -  Active: The connection has been activated.                                                                                                         |
      |                 |                 |                 | -  DELETED: The connection has been deleted.                                                                                                          |
      |                 |                 |                 |                                                                                                                                                       |
      |                 |                 |                 | .. note::                                                                                                                                             |
      |                 |                 |                 |                                                                                                                                                       |
      |                 |                 |                 |    The connection status is case insensitive.                                                                                                         |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
      | name            | No              | String          | Connection name                                                                                                                                       |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
      | tags            | No              | String          | List of tag names. The value is **k=v** for a single tag. Multiple tags are separated by commas (,). Example: **tag1=v1,tag2=v2**.                    |
      +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      GET /v2.0/{project_id}/datasource/enhanced-connections?limit=\ *{limit}*\ &offset=\ *{offset}*\ &status=\ *{status}*\ &name=\ *{name}*

Request Parameters
------------------

None

Response Parameters
-------------------

.. table:: **Table 3** Response parameters

   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type             | Description                                                                                                                 |
   +=============+===========+==================+=============================================================================================================================+
   | is_success  | No        | Boolean          | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | message     | No        | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | connections | No        | Array of objects | Datasource connection information list. For details, see :ref:`Table 4 <dli_02_0190__table252911713480>`.                   |
   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | count       | No        | Integer          | Number of returned datasource connections.                                                                                  |
   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0190__table252911713480:

.. table:: **Table 4** **connections** parameters

   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter            | Mandatory       | Type             | Description                                                                                                                                                                                                         |
   +======================+=================+==================+=====================================================================================================================================================================================================================+
   | id                   | No              | String           | Connection ID. Identifies the UUID of a datasource connection.                                                                                                                                                      |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | name                 | No              | String           | User-defined connection name.                                                                                                                                                                                       |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | status               | No              | String           | Connection status. The options are as follows:                                                                                                                                                                      |
   |                      |                 |                  |                                                                                                                                                                                                                     |
   |                      |                 |                  | -  Active: The connection has been activated.                                                                                                                                                                       |
   |                      |                 |                  | -  DELETED: The connection has been deleted.                                                                                                                                                                        |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | available_queue_info | No              | Array of objects | For details about how to create a datasource connection for each queue, see :ref:`Table 5 <dli_02_0190__table9559942155012>`.                                                                                       |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dest_vpc_id          | No              | String           | The VPC ID of the connected service.                                                                                                                                                                                |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dest_network_id      | No              | String           | Subnet ID of the connected service.                                                                                                                                                                                 |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | isPrivis             | No              | Boolean          | Whether the project permissions have been granted for the enhanced datasource connection. If the datasource connection has the permissions, the value of this field is **false**. Otherwise, the value is **true**. |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | create_time          | No              | Long             | Time when a link is created. The time is converted to a UTC timestamp.                                                                                                                                              |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | hosts                | No              | Array of objects | User-defined host information. For details, see :ref:`Table 7 <dli_02_0190__table6991727151310>`.                                                                                                                   |
   +----------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0190__table9559942155012:

.. table:: **Table 5** available_queue_info parameter description

   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type   | Description                                                                                                  |
   +=============+===========+========+==============================================================================================================+
   | peer_id     | No        | String | ID of a datasource connection.                                                                               |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | status      | No        | String | Connection status. For details about the status code, see :ref:`Table 8 <dli_02_0190__table13946174752513>`. |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | name        | No        | String | Name of a queue.                                                                                             |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | err_msg     | No        | String | Detailed error message when the status is **FAILED**.                                                        |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | update_time | No        | Long   | Time when the available queue list was updated.                                                              |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

.. table:: **Table 6** elastic_resource_pools parameters

   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type   | Description                                                                                                  |
   +=============+===========+========+==============================================================================================================+
   | peer_id     | No        | String | ID of a datasource connection.                                                                               |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | status      | No        | String | Connection status. For details about the status code, see :ref:`Table 8 <dli_02_0190__table13946174752513>`. |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | name        | No        | String | Elastic resource pool name                                                                                   |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | err_msg     | No        | String | Detailed error message when the status is **FAILED**.                                                        |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | update_time | No        | Long   | Time when the available queue list was updated.                                                              |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

.. _dli_02_0190__table6991727151310:

.. table:: **Table 7** **hosts** parameters

   ========= ========= ====== ========================
   Parameter Mandatory Type   Description
   ========= ========= ====== ========================
   name      No        String Custom host name
   ip        No        String IPv4 address of the host
   ========= ========= ====== ========================

.. _dli_02_0190__table13946174752513:

.. table:: **Table 8** Connection status

   +-----------+------------+------------------------------------------------------------------------------------------------------+
   | Parameter | Definition | Description                                                                                          |
   +===========+============+======================================================================================================+
   | CREATING  | Creating   | The datasource connection is being created.                                                          |
   +-----------+------------+------------------------------------------------------------------------------------------------------+
   | ACTIVE    | Active     | The datasource connection has been created, and the connection to the destination address is normal. |
   +-----------+------------+------------------------------------------------------------------------------------------------------+
   | FAILED    | Failed     | Failed to create a datasource connection.                                                            |
   +-----------+------------+------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "count": 1,
     "connections": [
       {
         "name": "withvpc",
         "id": "4c693ecc-bab8-4113-a838-129cedc9a563",
         "available_queue_info": [
           {
             "status": "ACTIVE",
             "name": "resource_mode_1",
             "peer_id": "d2ae6628-fa37-4e04-806d-c59c497492d1",
             "err_msg": "",
             "update_time": 1566889577861
           }
         ],
         "dest_vpc_id": "22094d8f-c310-4621-913d-4c4d655d8495",
         "dest_network_id": "78f2562a-36e4-4b39-95b9-f5aab22e1281",
         "isPrivis": true,
         "create_time": 1566888011125,
         "status": "ACTIVE"
       }
     ]
   }

Status Codes
------------

:ref:`Table 9 <dli_02_0190__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0190__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 9** Status codes

   =========== ========================
   Status Code Description
   =========== ========================
   200         The query is successful.
   400         Request error.
   500         Internal service error.
   =========== ========================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
