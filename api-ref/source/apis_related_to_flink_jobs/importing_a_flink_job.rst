:original_name: dli_02_0255.html

.. _dli_02_0255:

Importing a Flink Job
=====================

Function
--------

This API is used to import Flink job data.

URI
---

-  URI format

   POST /v1.0/{project_id}/streaming/jobs/import

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

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                  |
   +=================+=================+=================+==============================================================================================================================+
   | zip_file        | Yes             | String          | Path of the job ZIP file imported from OBS. You can enter a folder path to import all ZIP files in the folder.               |
   |                 |                 |                 |                                                                                                                              |
   |                 |                 |                 | .. note::                                                                                                                    |
   |                 |                 |                 |                                                                                                                              |
   |                 |                 |                 |    The folder can contain only **.zip** files.                                                                               |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+
   | is_cover        | No              | Boolean         | Whether to overwrite an existing job if the name of the imported job is the same as that of the existing job in the service. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type             | Description                                                                                                                 |
   +=============+===========+==================+=============================================================================================================================+
   | is_success  | No        | Boolean          | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | message     | No        | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                              |
   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+
   | job_mapping | No        | Array of Objects | Information about the imported job. For details, see :ref:`Table 4 <dli_02_0255__table9244145865320>`.                      |
   +-------------+-----------+------------------+-----------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0255__table9244145865320:

.. table:: **Table 4** **job_mapping** parameter description

   +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type   | Description                                                                                                                                                               |
   +============+===========+========+===========================================================================================================================================================================+
   | old_job_id | No        | Long   | ID of a job before being imported.                                                                                                                                        |
   +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | new_job_id | No        | Long   | ID of a job after being imported. If **is_cover** is set to **false** and a job with the same name exists in the service, the returned value of this parameter is **-1**. |
   +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | remark     | No        | String | Results about an imported job.                                                                                                                                            |
   +------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

.. code-block::

   {
       "zip_file": "test/ggregate_1582677879475.zip",
       "is_cover": true
   }

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "The job is imported successfully.",
       "job_mapping": [
           {
               "old_job_id": "100",
               "new_job_id": "200",
               "remark": "Job successfully created"
           }
       ]
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0255__t43c1f1c0ba344f4cbcb270953d9cca2a>` describes status codes.

.. _dli_02_0255__t43c1f1c0ba344f4cbcb270953d9cca2a:

.. table:: **Table 5** Status codes

   =========== =================================
   Status Code Description
   =========== =================================
   200         The job is imported successfully.
   400         The input parameter is invalid.
   =========== =================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
