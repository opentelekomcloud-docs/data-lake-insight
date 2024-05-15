:original_name: dli_02_0022.html

.. _dli_02_0022:

Querying Job Details
====================

Function
--------

This API is used to query details about jobs, including **databasename**, **tablename**, **file size**, and **export mode**.

URI
---

-  URI format

   GET/v1.0/{project_id}/jobs/{job_id}/detail

-  Parameter description

   .. table:: **Table 1** URI parameters

      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter  | Mandatory | Type   | Description                                                                                                                                   |
      +============+===========+========+===============================================================================================================================================+
      | project_id | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | job_id     | Yes       | String | Job ID. You can get the value by calling :ref:`Submitting a SQL Job (Recommended) <dli_02_0102>`.                                             |
      +------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory       | Type             | Description                                                                                                                                                                                   |
   +====================+=================+==================+===============================================================================================================================================================================================+
   | is_success         | Yes             | Boolean          | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed.                                                                             |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | message            | Yes             | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                                                                                                |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | job_id             | Yes             | String           | Job ID.                                                                                                                                                                                       |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | owner              | Yes             | String           | User who submits a job.                                                                                                                                                                       |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_time         | Yes             | Long             | Time when a job is started. The timestamp is in milliseconds.                                                                                                                                 |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | duration           | Yes             | Long             | Duration for executing the job (unit: millisecond).                                                                                                                                           |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | export_mode        | No              | String           | Specified export mode during data export and query result saving.                                                                                                                             |
   |                    |                 |                  |                                                                                                                                                                                               |
   |                    |                 |                  | Available values are **ErrorIfExists** and **Overwrite**.                                                                                                                                     |
   |                    |                 |                  |                                                                                                                                                                                               |
   |                    |                 |                  | -  **ErrorIfExists**: Ensure that the specified export directory does not exist. If the specified export directory exists, an error is reported and the export operation cannot be performed. |
   |                    |                 |                  | -  **Overwrite**: If you add new files to a specific directory, existing files will be deleted.                                                                                               |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_path          | Yes             | String           | Path to imported or exported files.                                                                                                                                                           |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_type          | Yes             | String           | Type of data to be imported or exported. Currently, only CSV and JSON are supported.                                                                                                          |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database_name      | Yes             | String           | Name of the database where the table, where data is imported or exported, resides.                                                                                                            |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name         | Yes             | String           | Name of the table where data is imported or exported.                                                                                                                                         |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | with_column_header | No              | Boolean          | Whether the imported data contains the column name during the execution of an import job.                                                                                                     |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | delimiter          | No              | String           | User-defined data delimiter set when the import job is executed.                                                                                                                              |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote_char         | No              | String           | User-defined quotation character set when the import job is executed.                                                                                                                         |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | escape_char        | No              | String           | User-defined escape character set when the import job is executed.                                                                                                                            |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | date_format        | No              | String           | Table date format specified when the import job is executed.                                                                                                                                  |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | timestamp_format   | No              | String           | Table time format specified when the import job is executed.                                                                                                                                  |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | compress           | No              | String           | Compression mode specified when the export job is executed.                                                                                                                                   |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tags               | No              | Array of objects | Job tags. For details, see :ref:`Table 3 <dli_02_0022__table9391124139>`.                                                                                                                     |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0022__table9391124139:

.. table:: **Table 3** tags parameter

   ========= ========= ====== ===========
   Parameter Mandatory Type   Description
   ========= ========= ====== ===========
   key       Yes       String Tag key
   value     Yes       String Tag value
   ========= ========= ====== ===========

Example Request
---------------

None

Example Response
----------------

-  Querying jobs of the **Import** type

   .. code-block::

      {
        "is_success": true,
        "message": "",
        "data_path": "obs://DLI/computeCharging/test.csv",
        "data_type": "json",
        "database_name": "iam_exist",
        "date_format": "yyyy-MM-dd",
        "delimiter": ",",
        "duration": 1623,
        "escape_char": "\\",
        "job_id": "a85d7298-ecef-47f9-bb31-499d2099d112",
        "owner": "iam_exist",
        "quote_char": "\"",
        "start_time": 1517385246111,
        "table_name": "DLI_table20",
        "timestamp_format": "yyyy-MM-dd HH:mm:ss",
        "with_column_header": false
      }

-  Query jobs of the **Export** type

   .. code-block::

      {
        "is_success": true,
        "message": "",
        "compress": "none",
        "data_path": "obs://xxx/dli/path6",
        "data_type": "json",
        "database_name": "submitjob",
        "duration": 4142,
        "export_mode": "Overwrite",
        "job_id": "b89fccb2-de6a-4c6c-b9b2-21f08a2eb85e",
        "owner": "test",
        "start_time": 1524107798024,
        "table_name": "autotest"
      }

Status Codes
------------

:ref:`Table 4 <dli_02_0022__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0022__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

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
