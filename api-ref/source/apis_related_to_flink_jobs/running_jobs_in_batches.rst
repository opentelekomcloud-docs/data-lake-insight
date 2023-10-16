:original_name: dli_02_0233.html

.. _dli_02_0233:

Running Jobs in Batches
=======================

Function
--------

This API is used to trigger batch job running.

URI
---

-  URI format

   POST /v1.0/{project_id}/streaming/jobs/run

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------+
   | Parameter        | Mandatory       | Type            | Description                                                                                                  |
   +==================+=================+=================+==============================================================================================================+
   | job_ids          | Yes             | Array of Long   | Batch job ID. You can obtain the job ID by calling the API for creating a job or the API for querying a job. |
   +------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------+
   | resume_savepoint | No              | Boolean         | Whether to restore a job from the latest savepoint.                                                          |
   |                  |                 |                 |                                                                                                              |
   |                  |                 |                 | -  If **resume_savepoint** is set to **true**, the job is restored from the latest savepoint.                |
   |                  |                 |                 | -  If **resume_savepoint** is set to **false**, the job is started normally, not from a specific savepoint.  |
   |                  |                 |                 |                                                                                                              |
   |                  |                 |                 | The default value is **false**.                                                                              |
   +------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +----------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type             | Description                                                                                                                    |
   +================+===========+==================+================================================================================================================================+
   | Array elements | No        | Array of Objects | The response message returned is as follows: For details, see :ref:`Table 4 <dli_02_0233__t5995d65f65ba4ebca8606202112b407e>`. |
   +----------------+-----------+------------------+--------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0233__t5995d65f65ba4ebca8606202112b407e:

.. table:: **Table 4** Array element parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | Message content.                                                                                                  |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Run the jobs whose IDs are **131**, **130**, **138**, and **137** and allow the jobs to be restored from their latest savepoints.

.. code-block::

   {
       "job_ids": [131,130,138,137],
       "resume_savepoint": true
   }

Example Response
----------------

.. code-block::

   [
       {
           "is_success": "true",
           "message": "The request for submitting DLI jobs is delivered successfully."
       },
       {
           "is_success": "true",
           "message": "The request for submitting DLI jobs is delivered successfully."
       },
       {
           "is_success": "true",
           "message": "The request for submitting DLI jobs is delivered successfully."
       },
       {
           "is_success": "true",
           "message": "The request for submitting DLI jobs is delivered successfully."
       }
   ]

Status Codes
------------

:ref:`Table 5 <dli_02_0233__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0233__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 5** Status codes

   =========== =====================================
   Status Code Description
   =========== =====================================
   200         Jobs are successfully run in batches.
   400         The input parameter is invalid.
   =========== =====================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
