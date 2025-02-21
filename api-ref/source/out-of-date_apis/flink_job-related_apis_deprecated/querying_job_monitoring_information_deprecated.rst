:original_name: dli_02_0238.html

.. _dli_02_0238:

Querying Job Monitoring Information (Deprecated)
================================================

Function
--------

This API is used to query job monitoring information. You can query monitoring information about multiple jobs at the same time.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   POST /v1.0/{project_id}/streaming/jobs/metrics

-  Parameter description

   .. table:: **Table 1** URI parameter

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameter

   ========= ========= ============= ================
   Parameter Mandatory Type          Description
   ========= ========= ============= ================
   job_ids   Yes       Array of Long List of job IDs.
   ========= ========= ============= ================

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+---------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                       |
   +============+===========+=========+===================================================================================================+
   | is_success | No        | Boolean | Indicates whether the request is successful.                                                      |
   +------------+-----------+---------+---------------------------------------------------------------------------------------------------+
   | message    | No        | String  | Message content.                                                                                  |
   +------------+-----------+---------+---------------------------------------------------------------------------------------------------+
   | metrics    | No        | Object  | Information about a job list. For details, see :ref:`Table 4 <dli_02_0238__table19266168121016>`. |
   +------------+-----------+---------+---------------------------------------------------------------------------------------------------+

.. _dli_02_0238__table19266168121016:

.. table:: **Table 4** **payload** parameters

   +-----------+-----------+------------------+----------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type             | Description                                                                                              |
   +===========+===========+==================+==========================================================================================================+
   | jobs      | No        | Array of objects | Monitoring information about all jobs. For details, see :ref:`Table 5 <dli_02_0238__table970234313114>`. |
   +-----------+-----------+------------------+----------------------------------------------------------------------------------------------------------+

.. _dli_02_0238__table970234313114:

.. table:: **Table 5** **jobs** parameters

   +-----------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                               |
   +===========+===========+========+===========================================================================================================================+
   | job_id    | No        | Long   | Job ID.                                                                                                                   |
   +-----------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------+
   | metrics   | No        | Object | All input and output monitoring information about a job. For details, see :ref:`Table 6 <dli_02_0238__table92501265155>`. |
   +-----------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0238__table92501265155:

.. table:: **Table 6** **metrics** parameters

   +------------------+-----------+------------------+-----------------------------------------------------------------------------------------+
   | Parameter        | Mandatory | Type             | Description                                                                             |
   +==================+===========+==================+=========================================================================================+
   | sources          | No        | Array of objects | All source streams. For details, see :ref:`Table 7 <dli_02_0238__table17208224152313>`. |
   +------------------+-----------+------------------+-----------------------------------------------------------------------------------------+
   | sinks            | No        | Array of objects | All sink streams. For details, see :ref:`Table 7 <dli_02_0238__table17208224152313>`.   |
   +------------------+-----------+------------------+-----------------------------------------------------------------------------------------+
   | total_read_rate  | No        | Double           | Total read rate.                                                                        |
   +------------------+-----------+------------------+-----------------------------------------------------------------------------------------+
   | total_write_rate | No        | Double           | Total write rate.                                                                       |
   +------------------+-----------+------------------+-----------------------------------------------------------------------------------------+

.. _dli_02_0238__table17208224152313:

.. table:: **Table 7** **source/sinks** parameters

   ================= ========= ====== ==================================
   Parameter         Mandatory Type   Description
   ================= ========= ====== ==================================
   name              No        String Name of the source or sink stream.
   records           No        Long   Total number of records.
   corrupted_records No        Long   Number of dirty data records.
   ================= ========= ====== ==================================

Example
-------

-  Example request

   .. code-block::

      {
          "job_ids": [298765, 298766]
      }

-  Example response

   .. code-block::

      {
          "is_success": true,
          "message": "Message content",
          "metrics": {
              "jobs": [
                  {
                      "job_id": 0,
                      "metrics": {
                          "sources": [
                              {
                                  "name": "Source: KafKa_6070_KAFKA_SOURCE",
                                  "records": 0,
                                  "corrupted_records": 0
                              }
                          ],
                          "sinks": [
                              {
                                  "name": "Source: KafKa_6070_KAFKA_SOURCE",
                                  "records": 0,
                                  "corrupted_records": 0
                              }
                          ],
                          "total_read_rate": 100,
                          "total_write_rate": 100
                      }
                  }
              ]
          }
      }

Status Codes
------------

.. table:: **Table 8** Status codes

   =========== =================================================
   Status Code Description
   =========== =================================================
   200         The query of job monitoring information succeeds.
   400         The input parameter is invalid.
   =========== =================================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
