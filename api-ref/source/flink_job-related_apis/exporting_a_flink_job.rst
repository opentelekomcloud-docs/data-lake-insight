:original_name: dli_02_0254.html

.. _dli_02_0254:

Exporting a Flink Job
=====================

Function
--------

This API is used to export Flink job data.

URI
---

-  URI format

   POST /v1.0/{project_id}/streaming/jobs/export

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

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                       |
   +=================+=================+=================+===================================================================================================+
   | obs_dir         | Yes             | String          | OBS path for storing exported job files.                                                          |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------+
   | is_selected     | Yes             | Boolean         | Whether to export a specified job.                                                                |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------+
   | job_selected    | No              | Array of Longs  | This parameter indicates the ID set of jobs to be exported if **is_selected** is set to **true**. |
   |                 |                 |                 |                                                                                                   |
   |                 |                 |                 | .. note::                                                                                         |
   |                 |                 |                 |                                                                                                   |
   |                 |                 |                 |    This parameter is mandatory when **is_selected** is set to **true**.                           |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type             | Description                                                                                                                 |
   +============+===========+==================+=============================================================================================================================+
   | is_success | No        | Boolean          | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | zip_file   | No        | Array of Strings | Name of the ZIP package containing exported jobs. The ZIP package is stored on OBS.                                         |
   +------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

Export the job whose ID is **100** to OBS.

.. code-block::

   {
       "obs_dir": "obs-test",
       "is_selected": true,
       "job_selected": [100]
   }

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "The job is exported successfully.",
       "zip_file": ["obs-test/aggregate_1582677879475.zip"]
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0254__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0254__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 4** Status codes

   =========== =================================
   Status Code Description
   =========== =================================
   200         The job is exported successfully.
   400         The input parameter is invalid.
   =========== =================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
