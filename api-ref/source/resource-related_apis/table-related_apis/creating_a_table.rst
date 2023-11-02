:original_name: dli_02_0034.html

.. _dli_02_0034:

Creating a Table
================

Function
--------

This API is used to create a table.

.. note::

   This API is a synchronous API.

URI
---

-  URI format

   POST /v1.0/{project_id}/databases/{database_name}/tables

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of the database where the new table resides.                                                                                             |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

.. table:: **Table 2** Request parameters

   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory       | Type             | Description                                                                                                                                                                                                                                                                                                |
   +====================+=================+==================+============================================================================================================================================================================================================================================================================================================+
   | table_name         | Yes             | String           | Name of the created table.                                                                                                                                                                                                                                                                                 |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                            |
   |                    |                 |                  | -  The table name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_).                                                                                                                                                                   |
   |                    |                 |                  | -  The table name is case insensitive and cannot be left unspecified.                                                                                                                                                                                                                                      |
   |                    |                 |                  | -  The table name can contain the dollar sign ($). Example: **$test**                                                                                                                                                                                                                                      |
   |                    |                 |                  | -  The length of the database name cannot exceed 128 characters.                                                                                                                                                                                                                                           |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_location      | Yes             | String           | Location where data is stored. The options are as follows:                                                                                                                                                                                                                                                 |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                            |
   |                    |                 |                  | -  OBS: OBS table                                                                                                                                                                                                                                                                                          |
   |                    |                 |                  | -  DLI: DLI table                                                                                                                                                                                                                                                                                          |
   |                    |                 |                  | -  VIEW: VIEW table                                                                                                                                                                                                                                                                                        |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | description        | No              | String           | Information about the new table.                                                                                                                                                                                                                                                                           |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | columns            | Yes             | Array of Objects | Columns of the new table. For details about column parameters, see :ref:`Table 4 <dli_02_0034__table985381581217>`. This parameter is optional when **data_location** is **VIEW**.                                                                                                                         |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement   | No              | String           | Query statement required for creating a view. The database to which the table belongs needs to be specified in the query statement, in the format of *database*.\ *table*. This parameter is mandatory when **data_location** is **VIEW**.                                                                 |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_type          | No              | String           | Type of the data to be added to the OBS table. The options are as follows: Parquet, ORC, CSV, JSON, and Avro.                                                                                                                                                                                              |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                            |
   |                    |                 |                  | .. note::                                                                                                                                                                                                                                                                                                  |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                            |
   |                    |                 |                  |    This parameter is mandatory for an OBS table.                                                                                                                                                                                                                                                           |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_path          | No              | String           | Storage path of data in the new OBS table, which must be a path on OBS and must begin with **obs**.                                                                                                                                                                                                        |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                            |
   |                    |                 |                  | .. note::                                                                                                                                                                                                                                                                                                  |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                            |
   |                    |                 |                  |    This parameter is mandatory for an OBS table.                                                                                                                                                                                                                                                           |
   |                    |                 |                  |                                                                                                                                                                                                                                                                                                            |
   |                    |                 |                  |    Do not set this parameter to the OBS root directory. Otherwise, all data in the root directory will be cleared when you clear table data.                                                                                                                                                               |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | with_column_header | No              | Boolean          | Whether the table header is included in the OBS table data. Only data in CSV files has this attribute. This parameter is mandatory when **data_location** is **OBS**.                                                                                                                                      |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | delimiter          | No              | String           | User-defined data delimiter. Only data in CSV files has this attribute. This parameter is mandatory when **data_location** is **OBS**.                                                                                                                                                                     |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote_char         | No              | String           | User-defined reference character. Double quotation marks ("\\") are used by default. Only data in CSV files has this attribute. This parameter is mandatory when **data_location** is **OBS**.                                                                                                             |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | escape_char        | No              | String           | User-defined escape character. Backslashes (\\\\) are used by default. Only data in CSV files has this attribute. This parameter is mandatory when **data_location** is **OBS**.                                                                                                                           |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | date_format        | No              | String           | User-defined date type. **yyyy-MM-dd** is used by default. For details about the characters involved in the date format, see :ref:`Table 3 <dli_02_0019__table489265920252>`. Only data in CSV and JSON files has this attribute. This parameter is mandatory when **data_location** is **OBS**.           |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | timestamp_format   | No              | String           | User-defined timestamp type. **yyyy-MM-dd HH:mm:ss** is used by default. For definitions about characters in the timestamp format, see :ref:`Table 3 <dli_02_0019__table489265920252>`. Only data in CSV and JSON files has this attribute. This parameter is mandatory when **data_location** is **OBS**. |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tags               | No              | Array of Objects | Database tag. For details about this object, see :ref:`tags parameters <dli_02_0034__table1769574233118>`.                                                                                                                                                                                                 |
   +--------------------+-----------------+------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0034__table1769574233118:

.. table:: **Table 3** tags parameters

   ========= ========= ====== ===========
   Parameter Mandatory Type   Description
   ========= ========= ====== ===========
   key       Yes       String Tag key
   value     Yes       String Tag value
   ========= ========= ====== ===========

.. _dli_02_0034__table985381581217:

.. table:: **Table 4** **columns** parameters

   +---------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter           | Mandatory       | Type            | Description                                                                                                                                                                          |
   +=====================+=================+=================+======================================================================================================================================================================================+
   | column_name         | Yes             | String          | Name of a column.                                                                                                                                                                    |
   +---------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | type                | Yes             | String          | Data type of a column.                                                                                                                                                               |
   +---------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | description         | No              | String          | Description of a column.                                                                                                                                                             |
   +---------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | is_partition_column | No              | Boolean         | Whether the column is a partition column. The value **true** indicates a partition column, and the value **false** indicates a non-partition column. The default value is **false**. |
   |                     |                 |                 |                                                                                                                                                                                      |
   |                     |                 |                 | .. note::                                                                                                                                                                            |
   |                     |                 |                 |                                                                                                                                                                                      |
   |                     |                 |                 |    When creating a partition table, ensure that at least one column in the table is a non-partition column. For details, see "Request example".                                      |
   +---------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Response
--------

.. table:: **Table 5** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

-  Create a table whose **data_location** is **OBS** and data format of CSV.

   .. code-block::

      {
        "table_name": "tb1",
        "data_location": "OBS",
        "description": "",
        "data_type": "csv",
        "data_path": "obs://obs/path1",
        "columns": [
        {
           "column_name": "column1",
           "type": "string",
           "description": "",
           "is_partition_column": true
        },
        {
           "column_name": "column2",
           "type": "string",
           "description": "",
           "is_partition_column": false
        }
        ],
        "with_column_header": true,
        "delimiter": ",",
        "quote_char": "\"",
        "escape_char": "\\",
        "date_format": "yyyy-MM-dd",
        "timestamp_format": "yyyy-MM-dd HH:mm:ss"
      }

   .. note::

      The values of **date_format** and **timestamp_format** must be the same as the time format in the imported CSV file.

-  Create a table whose **data_location** is **VIEW**.

   .. code-block::

      {
        "table_name": "view1",
        "data_location": "VIEW",
        "columns": [
        {
           "column_name": "column1",
           "type": "string",
           "description": "",
           "is_partition_column": true
        },
        {
           "column_name": "column2",
           "type": "string",
           "description": "",
           "is_partition_column": false
        }
        ],
        "select_statement": "select * from db1.tb1"
      }

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": ""
   }

Status Codes
------------

:ref:`Table 6 <dli_02_0034__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0034__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 6** Status codes

   =========== ================================
   Status Code Description
   =========== ================================
   200         The job is created successfully.
   400         Request error.
   500         Internal service error.
   =========== ================================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
