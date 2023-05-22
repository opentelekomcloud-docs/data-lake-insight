:original_name: dli_02_0285.html

.. _dli_02_0285:

Querying Connectivity Test Details of a Specified Address
=========================================================

Function
--------

This API is used to query the connectivity test result after the test is submitted.

URI
---

-  URI format

   GET /v1.0/{project_id}/queues/{queue_name}/connection-test/{task_id}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | queue_name | Yes       | String | Name of a queue.                                                                                                                              |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | task_id    | Yes       | String | Job ID. You can call :ref:`Creating an Address Connectivity Test Request <dli_02_0284>` to obtain the value.                                  |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter    | Mandatory | Type    | Description                                                                                                                 |
   +==============+===========+=========+=============================================================================================================================+
   | is_success   | Yes       | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message      | Yes       | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | connectivity | Yes       | String  | Indicates the connectivity test result.                                                                                     |
   +--------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "Get node connectivity status successfully for addressId:9",
       "connectivity": "REACHABLE"
   }

Status Codes
------------

:ref:`Table 3 <dli_02_0285__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0285__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 3** Status codes

   =========== ========================
   Status Code Description
   =========== ========================
   200         The query is successful.
   400         Request failure.
   500         Internal service error.
   =========== ========================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
