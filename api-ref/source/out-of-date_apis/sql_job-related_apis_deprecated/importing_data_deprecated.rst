:original_name: dli_02_0019.html

.. _dli_02_0019:

Importing Data (Deprecated)
===========================

Function
--------

This API is used to import data from a file to a DLI or OBS table. Currently, only OBS data can be imported to a DLI or OBS table.

.. note::

   -  This API has been deprecated and is not recommended.
   -  This API is asynchronous.
   -  When importing data, you can select an existing OBS bucket path or create an OBS bucket path, but only one OBS bucket path can be specified.
   -  If you need to create an OBS bucket, ensure that the bucket name complies with the following naming rules:

      -  The name must be globally unique in OBS.
      -  The name must contain 3 to 63 characters. Only lowercase letters, digits, hyphens (-), and periods (.) are allowed.
      -  The name cannot start or end with a period (.) or hyphen (-), and cannot contain two consecutive periods (.) or contain a period (.) and a hyphen (-) adjacent to each other.
      -  The name cannot be an IP address.
      -  If the name contains any period (.), the security certificate verification may be triggered when you access the bucket or objects in the bucket.

   -  If the type of a column in the source file to be imported does not match that of the target table, the query result of the row will be null.
   -  Two or more concurrent tasks of importing data to the same table are not allowed.

URI
---

-  URI format

   POST /v1.0/{project_id}/jobs/import-table

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

   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory       | Type             | Description                                                                                                                                                                                                                                                                                                     |
   +====================+=================+==================+=================================================================================================================================================================================================================================================================================================================+
   | data_path          | Yes             | String           | Path to the data to be imported. Currently, only OBS data can be imported.                                                                                                                                                                                                                                      |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_type          | Yes             | String           | Type of the data to be imported. Currently, data types of CSV, Parquet, ORC, JSON, and Avro are supported.                                                                                                                                                                                                      |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                                 |
   |                    |                 |                  | .. note::                                                                                                                                                                                                                                                                                                       |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                                 |
   |                    |                 |                  |    Data in **Avro** format generated by Hive tables cannot be imported.                                                                                                                                                                                                                                         |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database_name      | Yes             | String           | Name of the database where the table to which data is imported resides.                                                                                                                                                                                                                                         |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name         | Yes             | String           | Name of the table to which data is imported.                                                                                                                                                                                                                                                                    |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | with_column_header | No              | Boolean          | Whether the first line of the imported data contains column names, that is, headers. The default value is **false**, indicating that column names are not contained. This parameter can be specified when CSV data is imported.                                                                                 |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | delimiter          | No              | String           | User-defined data delimiter. The default value is a comma (,). This parameter can be specified when CSV data is imported.                                                                                                                                                                                       |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote_char         | No              | String           | User-defined quotation character. The default value is double quotation marks ("). This parameter can be specified when CSV data is imported.                                                                                                                                                                   |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | escape_char        | No              | String           | User-defined escape character. The default value is a backslash (\\). This parameter can be specified when CSV data is imported.                                                                                                                                                                                |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | date_format        | No              | String           | Specified date format. The default value is **yyyy-MM-dd**. For details about the characters involved in the date format, see :ref:`Table 3 <dli_02_0019__table489265920252>`. This parameter can be specified when data in the CSV or JSON format is imported.                                                 |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | bad_records_path   | No              | String           | **Bad records** storage directory during job execution. After configuring this item, the **bad records** is not imported into the target table.                                                                                                                                                                 |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | timestamp_format   | No              | String           | Specified time format. The default value is **yyyy-MM-dd HH:mm:ss**. For definitions about characters in the time format, see :ref:`Table 3 <dli_02_0019__table489265920252>`. This parameter can be specified when data in the CSV or JSON format is imported.                                                 |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queue_name         | No              | String           | Name of the queue that is specified to execute a task. If no queue is specified, the default queue is used.                                                                                                                                                                                                     |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | overwrite          | No              | Boolean          | Whether to overwrite data. The default value is **false**, indicating appending write. If the value is **true**, it indicates overwriting.                                                                                                                                                                      |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | partition_spec     | No              | Object           | Partition to which data is to be imported.                                                                                                                                                                                                                                                                      |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                                 |
   |                    |                 |                  | -  If this parameter is not set, the entire table data is dynamically imported. The imported data must contain the data in the partition column.                                                                                                                                                                |
   |                    |                 |                  | -  If this parameter is set and all partition information is configured during data import, data is imported to the specified partition. The imported data cannot contain data in the partition column.                                                                                                         |
   |                    |                 |                  | -  If not all partition information is configured during data import, the imported data must contain all non-specified partition data. Otherwise, abnormal values such as **null** exist in the partition field column of non-specified data after data import.                                                 |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | conf               | No              | Array of Strings | User-defined parameter that applies to the job. Currently, **dli.sql.dynamicPartitionOverwrite.enabled** can be set to **false** by default. If it is set to **true**, data in a specified partition is overwritten. If it is set to **false**, data in the entire DataSource table is dynamically overwritten. |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                                 |
   |                    |                 |                  | .. note::                                                                                                                                                                                                                                                                                                       |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                                 |
   |                    |                 |                  |    For dynamic overwrite of Hive partition tables, only the involved partition data can be overwritten. The entire table data cannot be overwritten.                                                                                                                                                            |
   +--------------------+-----------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0019__table489265920252:

.. table:: **Table 3** Definition of characters involved in the date and time patterns

   ========= ===================== =====================================
   Character Date or Time Element  Example
   ========= ===================== =====================================
   G         Epoch ID              AD
   y         Year                  1996; 96
   M         Month                 July; Jul; 07
   w         Which week in a year  27 (Week 27 in the year)
   W         Which week in a month 2 (Second week in the month)
   D         Which day in a year   189 (Day 189 in the year)
   d         Which day in a month  10 (Day 10 in the month)
   u         Which day in a week   1 (Monday), ..., 7 (Sunday)
   a         am/pm flag            pm (Afternoon)
   H         Hour time (0-23)      2
   h         Hour time (1-12)      12
   m         Minute time           30
   s         Second time           55
   S         Which milliseconds    978
   z         Time zone             Pacific Standard Time; PST; GMT-08:00
   ========= ===================== =====================================

Response
--------

.. table:: **Table 4** Response parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                      |
   +=================+=================+=================+==================================================================================================================================================+
   | is_success      | No              | Boolean         | Indicates whether the request is successfully sent. Value **true** indicates that the request is successfully sent.                              |
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

Import the CSV data stored on OBS to **db2.t2**.

.. code-block::

   {
       "data_path": "obs://home/data1/DLI/t1.csv",
       "data_type": "csv",
       "database_name": "db2",
       "table_name": "t2",
       "with_column_header": false,
       "delimiter": ",",
       "quote_char": ",",
       "escape_char": ",",
       "date_format": "yyyy-MM-dd",
       "timestamp_format": "yyyy-MM-dd'T'HH:mm:ss.SSSZZ",
       "queue_name": "queue2",
       "overwrite": false,
       "partition_spec":{
         "column1":  "2020-01-01",
         "column2":  "columnPartValue"
        }
   }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "import data to table t2 started",
     "job_id": "6b29eb77-4c16-4e74-838a-2cf7959e9202",
     "job_mode":"async"
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0019__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0019__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 5** Status codes

   =========== =======================
   Status Code Description
   =========== =======================
   200         Import succeeded.
   400         Request error.
   500         Internal service error.
   =========== =======================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
