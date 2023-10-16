:original_name: dli_02_0020.html

.. _dli_02_0020:

Exporting Data
==============

Function
--------

This API is used to export data from a DLI table to a file.

.. note::

   -  This API is asynchronous.
   -  Currently, data can be exported only from a DLI table to OBS, and the OBS path must be specified to the folder level. The OBS path cannot contain commas (,). The OBS bucket name cannot end with the regular expression format **.[0-9]+(.*)**. Specifically, if the bucket name contains dots (.), the last dot (.) cannot be followed by a digit, for example, **\**.12abc** and **\**.12**.
   -  Data can be exported across accounts. That is, after account B authorizes account A, account A can export data to the OBS path of account B if account A has the permission to read the metadata and permission information about the OBS bucket of account B and read and write the path.

URI
---

-  URI format

   POST /v1.0/{project_id}/jobs/export-table

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

   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory       | Type            | Description                                                                                                                                                                                                                               |
   +====================+=================+=================+===========================================================================================================================================================================================================================================+
   | data_path          | Yes             | String          | Path for storing the exported data. Currently, data can be stored only on OBS. If **export_mode** is set to **errorifexists**, the OBS path cannot contain the specified folder, for example, the **test** folder in the example request. |
   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_type          | Yes             | String          | Type of data to be exported. Currently, only CSV and JSON are supported.                                                                                                                                                                  |
   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database_name      | Yes             | String          | Name of the database where the table from which data is exported resides.                                                                                                                                                                 |
   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name         | Yes             | String          | Name of the table from which data is exported.                                                                                                                                                                                            |
   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | compress           | Yes             | String          | Compression mode for exported data. Currently, the compression modes **gzip**, **bzip2**, and **deflate** are supported. If you do not want to compress data, enter **none**.                                                             |
   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue_name         | No              | String          | Name of the queue that is specified to execute a task. If no queue is specified, the default queue is used.                                                                                                                               |
   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | export_mode        | No              | String          | Export mode. The parameter value can be **ErrorIfExists** or **Overwrite**. If **export_mode** is not specified, this parameter is set to **ErrorIfExists** by default.                                                                   |
   |                    |                 |                 |                                                                                                                                                                                                                                           |
   |                    |                 |                 | -  **ErrorIfExists**: Ensure that the specified export directory does not exist. If the specified export directory exists, an error is reported and the export operation cannot be performed.                                             |
   |                    |                 |                 | -  **Overwrite**: If you add new files to a specific directory, existing files will be deleted.                                                                                                                                           |
   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | with_column_header | No              | Boolean         | Whether to export column names when exporting CSV and JSON data.                                                                                                                                                                          |
   |                    |                 |                 |                                                                                                                                                                                                                                           |
   |                    |                 |                 | -  If this parameter is set to **true**, the column names are exported.                                                                                                                                                                   |
   |                    |                 |                 | -  If this parameter is set to **false**, the column names are not exported.                                                                                                                                                              |
   |                    |                 |                 | -  If this parameter is left blank, the default value **false** is used.                                                                                                                                                                  |
   +--------------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 3** Response parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                      |
   +=================+=================+=================+==================================================================================================================================================+
   | is_success      | No              | Boolean         | Whether the request is successfully sent. Value **true** indicates that the request is successfully sent.                                        |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | message         | No              | String          | System prompt. If execution succeeds, the parameter setting may be left blank.                                                                   |
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

Export data from **db2.t2** to OBS and store the data in JSON format.

.. code-block::

   {
       "data_path": "obs://home/data1/DLI/test",
       "data_type": "json",
       "database_name": "db2",
       "table_name": "t2",
       "compress": "gzip",
       "with_column_header": "true",
       "queue_name": "queue2"
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "export all data from table db2.t2 to path obs://home/data1/DLI/test started",
     "job_id": "828d4044-3d39-449b-b32c-957f7cfadfc9",
     "job_mode":"async"
   }

Status Codes
------------

:ref:`Table 4 <dli_02_0020__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0020__tb12870f1c5f24b27abd55ca24264af36:

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

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
