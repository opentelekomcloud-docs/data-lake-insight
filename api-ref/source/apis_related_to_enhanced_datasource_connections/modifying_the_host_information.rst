:original_name: dli_02_0200.html

.. _dli_02_0200:

Modifying the Host Information
==============================

Function
--------

This API is used to modify the host information of a connected datasource. Only full overwriting is supported.

URI
---

-  URI format

   PUT /v2.0/{project_id}/datasource/enhanced-connections/{connection_id}

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

.. table:: **Table 2** Request parameters

   +-----------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type             | Description                                                                                                                                                                                                                                         |
   +===========+===========+==================+=====================================================================================================================================================================================================================================================+
   | hosts     | Yes       | Array of objects | The user-defined host information. A maximum of 20,000 records are supported. For details, see :ref:`hosts request parameters <dli_02_0200__table6991727151310>`. If this parameter is left blank, all configured host information will be deleted. |
   +-----------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0200__table6991727151310:

.. table:: **Table 3** hosts request parameters

   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                                                                 |
   +===========+===========+========+=============================================================================================================================================================================+
   | name      | No        | String | The user-defined host name. The value can consist of 128 characters, including digits, letters, underscores (_), hyphens (-), and periods (.). It must start with a letter. |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ip        | No        | String | The IPv4 address of the host.                                                                                                                                               |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 4** Response parameters

   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Type    | Description                                                                                                                 |
   +============+=========+=============================================================================================================================+
   | is_success | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | String  | System prompt. If execution succeeds, the message may be left blank.                                                        |
   +------------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Modify the host information of an enhanced datasource connection.

.. code-block::

   {
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

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": ""
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0200__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0200__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 5** Status codes

   =========== ===========================================
   Status Code Description
   =========== ===========================================
   200         The modification operations are successful.
   400         Request error.
   500         Internal service error.
   =========== ===========================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
