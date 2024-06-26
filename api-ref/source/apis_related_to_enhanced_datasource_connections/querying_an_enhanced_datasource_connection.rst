:original_name: dli_02_0189.html

.. _dli_02_0189:

Querying an Enhanced Datasource Connection
==========================================

Function
--------

This API is used to query a created enhanced datasource connection.

URI
---

-  URI format

   GET /v2.0/{project_id}/datasource/enhanced-connections/{connection_id}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | connection_id | Yes       | String | Connection ID. Identifies the UUID of a datasource connection.                                                                                |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Parameter            | Mandatory       | Type             | Description                                                                                                                   |
   +======================+=================+==================+===============================================================================================================================+
   | is_success           | No              | Boolean          | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed.   |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | message              | No              | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                                |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | id                   | No              | String           | Connection ID. Identifies the UUID of a datasource connection.                                                                |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | name                 | No              | String           | User-defined connection name.                                                                                                 |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | status               | No              | String           | Connection status. The options are as follows:                                                                                |
   |                      |                 |                  |                                                                                                                               |
   |                      |                 |                  | -  Active: The connection has been activated.                                                                                 |
   |                      |                 |                  | -  DELETED: The connection has been deleted.                                                                                  |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | available_queue_info | No              | Array of objects | For details about how to create a datasource connection for each queue, see :ref:`Table 3 <dli_02_0189__table9559942155012>`. |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | dest_vpc_id          | No              | String           | The VPC ID of the connected service.                                                                                          |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | dest_network_id      | No              | String           | Subnet ID of the connected service.                                                                                           |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | create_time          | No              | Long             | Time when a link is created. The time is converted to a UTC timestamp.                                                        |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | hosts                | No              | Array of objects | User-defined host information. For details, see :ref:`hosts parameter description <dli_02_0189__table6991727151310>`.         |
   +----------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0189__table9559942155012:

.. table:: **Table 3** available_queue_info parameter description

   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type   | Description                                                                                                  |
   +=============+===========+========+==============================================================================================================+
   | peer_id     | No        | String | ID of a datasource connection.                                                                               |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | status      | No        | String | Connection status. For details about the status code, see :ref:`Table 5 <dli_02_0189__table13946174752513>`. |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | name        | No        | String | Name of a queue.                                                                                             |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | err_msg     | No        | String | Detailed error message when the status is **FAILED**.                                                        |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+
   | update_time | No        | Long   | Time when the available queue list was updated.                                                              |
   +-------------+-----------+--------+--------------------------------------------------------------------------------------------------------------+

.. _dli_02_0189__table6991727151310:

.. table:: **Table 4** hosts parameter description

   ========= ========= ====== =============================
   Parameter Mandatory Type   Description
   ========= ========= ====== =============================
   name      No        String The user-defined host name.
   ip        No        String The IPv4 address of the host.
   ========= ========= ====== =============================

.. _dli_02_0189__table13946174752513:

.. table:: **Table 5** Connection status

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
     "create_time": 1566888011125,
     "status": "ACTIVE",
     "hosts": [
       {
         "ip":"192.168.0.1",
         "name":"ecs-97f8-0001"
       },
       {
         "ip":"192.168.0.2",
         "name":"ecs-97f8-0002"
       }
     ]
   }

Status Codes
------------

:ref:`Table 6 <dli_02_0189__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0189__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 6** Status codes

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
