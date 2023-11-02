:original_name: dli_02_0195.html

.. _dli_02_0195:

Deleting a Queue
================

Function
--------

This API is used to delete a specified queue.

.. note::

   If a task is being executed in a specified queue, the queue cannot be deleted.

URI
---

-  URI format

   DELETE /v1.0/{project_id}/queues/{queue_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | queue_name | Yes       | String | Name of a queue to be deleted.                                                                                                                |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

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

:ref:`Table 3 <dli_02_0195__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0195__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 3** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Deletion succeeded.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
