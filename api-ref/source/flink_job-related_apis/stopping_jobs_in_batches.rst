:original_name: dli_02_0241.html

.. _dli_02_0241:

Stopping Jobs in Batches
========================

Function
--------

This API is used to stop running jobs in batches.

URI
---

-  URI format

   POST /v1.0/{project_id}/streaming/jobs/stop

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +-------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------+
   | Parameter         | Mandatory       | Type            | Description                                                                                                               |
   +===================+=================+=================+===========================================================================================================================+
   | job_ids           | Yes             | Array of Long   | Job ID.                                                                                                                   |
   +-------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------+
   | trigger_savepoint | No              | Boolean         | Whether to create a savepoint for a job to store the job status information before stopping it. The data type is Boolean. |
   |                   |                 |                 |                                                                                                                           |
   |                   |                 |                 | -  If this parameter is set to **true**, a savepoint is created.                                                          |
   |                   |                 |                 | -  If this parameter is set to **false**, no savepoint is created. The default value is **false**.                        |
   +-------------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +----------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type             | Description                                                                                                                    |
   +================+===========+==================+================================================================================================================================+
   | Array elements | No        | Array of objects | The response message returned is as follows: For details, see :ref:`Table 4 <dli_02_0241__t5995d65f65ba4ebca8606202112b407e>`. |
   +----------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0241__t5995d65f65ba4ebca8606202112b407e:

.. table:: **Table 4** Array element parameters

   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                       |
   +============+===========+========+===================================================================================================================+
   | is_success | No        | String | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String | Message content                                                                                                   |
   +------------+-----------+--------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Stop the jobs whose IDs are **128** and **137**.

.. code-block::

   {
     "job_ids": [128, 137],
     "trigger_savepoint": false
   }

Example Response
----------------

.. code-block::

   [{"is_success":"true",
   "message": "The request for stopping DLI jobs is delivered successfully."}]

Status Codes
------------

:ref:`Table 5 <dli_02_0241__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0241__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 5** Status codes

   =========== ===================================================
   Status Code Description
   =========== ===================================================
   200         The request of stopping a job is sent successfully.
   400         The input parameter is invalid.
   =========== ===================================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
