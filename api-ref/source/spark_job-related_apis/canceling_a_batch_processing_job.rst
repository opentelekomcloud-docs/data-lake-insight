:original_name: dli_02_0129.html

.. _dli_02_0129:

Canceling a Batch Processing Job
================================

Function
--------

This API is used to cancel a batch processing job.

.. note::

   Batch processing jobs in the **Successful** or **Failed** state cannot be canceled.

URI
---

-  URI format

   DELETE /v2.0/{project_id}/batches/{batch_id}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | batch_id   | Yes       | String | ID of a batch processing job. Set the value to the job ID obtained in :ref:`Creating a Batch Processing Job <dli_02_0124>`.                   |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameter

   +-----------+-----------+--------+--------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                          |
   +===========+===========+========+======================================================================================+
   | msg       | No        | String | If the batch processing job is successfully canceled, value **deleted** is returned. |
   +-----------+-----------+--------+--------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "msg": "deleted"
   }

Status Codes
------------

:ref:`Table 3 <dli_02_0129__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0129__tb12870f1c5f24b27abd55ca24264af36:

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
