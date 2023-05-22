:original_name: dli_02_0024.html

.. _dli_02_0024:

Exporting Query Results
=======================

Function
--------

This API is used to export results returned from the query using SQL statements to OBS. Only the query result of **QUERY** jobs can be exported.

.. note::

   -  This API is asynchronous.
   -  Currently, data can be exported only to OBS, and the OBS path must be specified to the folder level. The OBS path cannot contain commas (,). The OBS bucket name cannot end with the regular expression format ".[0-9]+(.*)". Specifically, if the bucket name contains dots (.), the last dot (.) cannot be followed by a digit, for example, "**.12abc" and "**.12".

URI
---

-  URI format

   POST /v1.0/{project_id}/jobs/{job_id}/export-result

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | job_id     | Yes       | String | Job ID.                                                                                                                                       |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory       | Type            | Description                                                                                                                                                                                   |
   +====================+=================+=================+===============================================================================================================================================================================================+
   | data_path          | Yes             | String          | Path for storing the exported data. Currently, data can be stored only on OBS. The OBS path cannot contain folders, for example, the **path** folder in the sample request.                   |
   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | compress           | No              | String          | Compression format of exported data. Currently, **gzip**, **bzip2**, and **deflate** are supported. The default value is **none**, indicating that data is not compressed.                    |
   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_type          | Yes             | String          | Storage format of exported data. Currently, only CSV and JSON are supported.                                                                                                                  |
   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue_name         | No              | String          | Name of the queue that is specified to execute a task. If no queue is specified, the default queue is used.                                                                                   |
   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | export_mode        | No              | String          | Export mode. The parameter value can be **ErrorIfExists** or **Overwrite**. If **export_mode** is not specified, this parameter is set to **ErrorIfExists** by default.                       |
   |                    |                 |                 |                                                                                                                                                                                               |
   |                    |                 |                 | -  **ErrorIfExists**: Ensure that the specified export directory does not exist. If the specified export directory exists, an error is reported and the export operation cannot be performed. |
   |                    |                 |                 | -  **Overwrite**: If you add new files to a specific directory, existing files will be deleted.                                                                                               |
   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | with_column_header | No              | Boolean         | Whether to export column names when exporting CSV and JSON data.                                                                                                                              |
   |                    |                 |                 |                                                                                                                                                                                               |
   |                    |                 |                 | -  If this parameter is set to **true**, the column names are exported.                                                                                                                       |
   |                    |                 |                 | -  If this parameter is set to **false**, the column names are not exported.                                                                                                                  |
   |                    |                 |                 | -  If this parameter is left blank, the default value **false** is used.                                                                                                                      |
   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | limit_num          | No              | Integer         | Number of data records to be exported. The default value is **0**, indicating that all data records are exported.                                                                             |
   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encoding_type      | No              | String          | Format of the data to be exported. The value can be **utf-8**, **gb2312**, or **gbk**. Value **utf-8** will be used if this parameter is left empty.                                          |
   +--------------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                      |
   +=================+=================+=================+==================================================================================================================================================+
   | is_success      | Yes             | Boolean         | Indicates whether the request is successfully sent. Value **true** indicates that the request is successfully sent.                              |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | message         | Yes             | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                                                   |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_id          | No              | String          | ID of a job returned after a job is generated and submitted by using SQL statements. The job ID can be used to query the job status and results. |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_mode        | No              | String          | Job execution mode. The options are as follows:                                                                                                  |
   |                 |                 |                 |                                                                                                                                                  |
   |                 |                 |                 | -  **async**: asynchronous                                                                                                                       |
   |                 |                 |                 | -  **sync**: synchronous                                                                                                                         |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

.. code-block::

   {
     "data_path": "obs://obs-bucket1/path",
     "data_type": "json",
     "compress": "gzip",
     "with_column_header": "true",
     "queue_name": "queue2",
     "limit_num": 10
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "job_id": "37a40ef9-86f5-42e6-b4c6-8febec89cc20",
     "job_mode":"async"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0024__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0024__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Export successful.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Code <dli_02_0056>`.
