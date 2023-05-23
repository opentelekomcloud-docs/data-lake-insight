:original_name: dli_02_0236.html

.. _dli_02_0236:

Querying the Job Execution Plan
===============================

Function
--------

This API is used to query a job execution plan.

URI
---

-  URI format

   GET /v1.0/{project_id}/streaming/jobs/{job_id}/execute-graph

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | job_id     | Yes       | Long   | Job ID.                                                                                                                                       |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +---------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------+
   | Parameter     | Mandatory | Type    | Description                                                                                                     |
   +===============+===========+=========+=================================================================================================================+
   | is_success    | No        | Boolean | Whether the request is successful.                                                                              |
   +---------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------+
   | message       | No        | String  | Message content.                                                                                                |
   +---------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------+
   | execute_graph | No        | Object  | Response parameter for querying a job plan. For details, see :ref:`Table 3 <dli_02_0236__table11948152164215>`. |
   +---------------+-----------+---------+-----------------------------------------------------------------------------------------------------------------+

.. _dli_02_0236__table11948152164215:

.. table:: **Table 3** **execute_graph** parameters

   =========== ========= ======= =============================
   Parameter   Mandatory Type    Description
   =========== ========= ======= =============================
   jid         No        String  ID of a Flink job.
   name        No        String  Name of a Flink job.
   isStoppable No        Boolean Whether a job can be stopped.
   state       No        String  Execution status of a job.
   start-time  No        Long    Time when a job is started.
   end-time    No        Long    Time when a job is stopped.
   duration    No        Long    Running duration of a job.
   =========== ========= ======= =============================

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": "true",
     "message": "Querying the job execution graph succeeds.",
       "execute_graph": {
           "jid": "4e966f43f2c90b0e1bf3188ecf55504b",
           "name": "",
           "isStoppable": false,
           "state": "RUNNING",
           "start-time": 1578904488436,
           "end-time": -1,
           "duration": 516274
       }
   }

Status Codes
------------

.. table:: **Table 4** Status codes

   =========== =========================================
   Status Code Description
   =========== =========================================
   200         Querying the job execution plan succeeds.
   400         The input parameter is invalid.
   =========== =========================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
