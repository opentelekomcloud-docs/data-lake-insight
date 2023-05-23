:original_name: dli_02_0294.html

.. _dli_02_0294:

Deleting a Scheduled CU Change
==============================

Function
--------

This API is used to delete a scheduled CU change for a queue with a specified ID.

URI
---

-  URI format

   DELETE /v1/{project_id}/queues/{queue_name}/plans/{plan_id}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`.                 |
      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | queue_name | Yes       | String | Name of the queue for which the scheduled CU change is to be deleted. The name contains 1 to 128 characters. Use commas (,) to separate multiple queue names. |
      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | plan_id    | Yes       | Long   | ID of scheduled CU change to be deleted. For details about how to obtain the IDs, see :ref:`Viewing a Scheduled CU Change <dli_02_0292>`.                     |
      +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                                 |
   +============+===========+=========+=============================================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": ""
   }

Status Codes
------------

:ref:`Table 3 <dli_02_0294__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0294__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 3** Status codes

   =========== ======================================
   Status Code Description
   =========== ======================================
   200         The directory is successfully deleted.
   400         Request failure.
   500         Internal service error.
   =========== ======================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.

.. table:: **Table 4** Error codes

   ========== ==================================
   Error Code Error Message
   ========== ==================================
   DLI.0002   The plan with id 8 does not exist.
   ========== ==================================
