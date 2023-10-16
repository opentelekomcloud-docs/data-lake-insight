:original_name: dli_02_0293.html

.. _dli_02_0293:

Deleting Scheduled CU Changes in Batches
========================================

Function
--------

This API is used to delete scheduled CU changes in batches.

URI
---

-  URI format

   POST /v1/{project_id}/queues/{queue_name}/plans/batch-delete

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`.                 |
      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | queue_name | Yes       | String | Name of the queue for which the scheduled CU change is to be deleted. The name contains 1 to 128 characters. Use commas (,) to separate multiple queue names. |
      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-----------+-----------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type          | Description                                                                                                                                           |
   +===========+===========+===============+=======================================================================================================================================================+
   | plan_ids  | Yes       | Array of Long | Scaling policy IDs of the queues you want to delete. For details, see :ref:`Viewing a Scheduled CU Change <dli_02_0292>`. Example: "plan_ids": [8,10] |
   +-----------+-----------+---------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                 |
   +============+===========+=========+=============================================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Delete the scaling plans whose IDs are 3 and 4.

.. code-block::

   {
      "plan_ids": [3,4]
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

:ref:`Table 4 <dli_02_0293__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0293__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Deletion succeeded.
   400         Request failure.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.

.. table:: **Table 5** Error codes

   ========== ====================================
   Error Code Error Message
   ========== ====================================
   DLI.0002   The plans with id 8, 9 do not exist.
   ========== ====================================
