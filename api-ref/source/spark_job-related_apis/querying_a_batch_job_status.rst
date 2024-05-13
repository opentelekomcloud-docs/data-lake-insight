:original_name: dli_02_0127.html

.. _dli_02_0127:

Querying a Batch Job Status
===========================

Function
--------

This API is used to obtain the execution status of a batch processing job.

URI
---

-  URI format

   GET /v2.0/{project_id}/batches/{batch_id}/state

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Description                                                                                                                                   |
      +============+===========+===============================================================================================================================================+
      | project_id | Yes       | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | batch_id   | Yes       | ID of a batch processing job.                                                                                                                 |
      +------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +-----------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                                                                          |
   +===========+===========+========+======================================================================================================================================================================================+
   | id        | No        | String | ID of a batch processing job, which is in the universal unique identifier (UUID) format.                                                                                             |
   +-----------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | state     | No        | String | Status of a batch processing job. For details, see :ref:`Table 7 <dli_02_0124__en-us_topic_0103343302_table16701351161919>` in :ref:`Creating a Batch Processing Job <dli_02_0124>`. |
   +-----------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {"id":"0a324461-d9d9-45da-a52a-3b3c7a3d809e","state":"Success"}

Status Codes
------------

:ref:`Table 3 <dli_02_0127__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0127__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 3** Status codes

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
