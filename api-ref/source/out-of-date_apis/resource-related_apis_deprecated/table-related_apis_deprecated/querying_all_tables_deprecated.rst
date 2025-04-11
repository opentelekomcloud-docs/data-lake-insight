:original_name: dli_02_0105.html

.. _dli_02_0105:

Querying All Tables (Deprecated)
================================

Function
--------

This API is used to query information about tables that meet the filtering criteria or all the tables in the specified database.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   GET /v1.0/{project_id}/databases/{database_name}/tables

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of the database where the table resides.                                                                                                 |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter         | Mandatory       | Type            | Description                                                                                                                                                  |
      +===================+=================+=================+==============================================================================================================================================================+
      | keyword           | No              | String          | Keywords used to filter table names.                                                                                                                         |
      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | with-detail       | No              | Boolean         | Whether to obtain detailed information about tables (such as owner and size). The default value is **false**.                                                |
      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | page-size         | No              | Integer         | Paging size. The minimum value is **1** and the maximum value is **100**.                                                                                    |
      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | current-page      | No              | Integer         | Current page number. The minimum value is **1**.                                                                                                             |
      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | with-priv         | No              | Boolean         | Whether to return permission information.                                                                                                                    |
      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | table-type        | No              | String          | Database table type. The options are as follows:                                                                                                             |
      |                   |                 |                 |                                                                                                                                                              |
      |                   |                 |                 | -  **MANAGED_TABLE**: DLI table                                                                                                                              |
      |                   |                 |                 | -  **EXTERNAL_TABLE**: OBS table                                                                                                                             |
      |                   |                 |                 | -  **VIRTUAL_VIEW**: view                                                                                                                                    |
      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | datasource-type   | No              | String          | Data source type. The options are as follows:                                                                                                                |
      |                   |                 |                 |                                                                                                                                                              |
      |                   |                 |                 | -  CloudTable                                                                                                                                                |
      |                   |                 |                 | -  CSS                                                                                                                                                       |
      |                   |                 |                 | -  DLI                                                                                                                                                       |
      |                   |                 |                 | -  GaussDB(DWS)                                                                                                                                              |
      |                   |                 |                 | -  Geomesa                                                                                                                                                   |
      |                   |                 |                 | -  HBase                                                                                                                                                     |
      |                   |                 |                 | -  JDBC                                                                                                                                                      |
      |                   |                 |                 | -  Mongo                                                                                                                                                     |
      |                   |                 |                 | -  OBS                                                                                                                                                       |
      |                   |                 |                 | -  ODPS                                                                                                                                                      |
      |                   |                 |                 | -  OpenTSDB                                                                                                                                                  |
      |                   |                 |                 | -  Redis                                                                                                                                                     |
      |                   |                 |                 | -  RDS                                                                                                                                                       |
      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | without-tablemeta | No              | Boolean         | Whether to obtain the metadata of a table. The default value is **false**. If this parameter is set to **true**, the response speed can be greatly improved. |
      +-------------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      The following is an example of the URL containing the **query** parameter:

      GET /v1.0/{project_id}/databases/{database_name}/tables\ *?keyword=tb&with-detail=true*

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Type             | Description                                                                                                       |
   +=============+===========+==================+===================================================================================================================+
   | is_success  | Yes       | Boolean          | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | message     | Yes       | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | table_count | Yes       | Integer          | Total number of tables.                                                                                           |
   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+
   | tables      | Yes       | Array of objects | Table information. For details, see :ref:`Table 4 <dli_02_0105__table6846515164814>`.                             |
   +-------------+-----------+------------------+-------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0105__table6846515164814:

.. table:: **Table 4** tables parameters

   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | Parameter         | Mandatory       | Type             | Description                                                                                                   |
   +===================+=================+==================+===============================================================================================================+
   | create_time       | Yes             | Long             | Time when a table is created. The timestamp is expressed in milliseconds.                                     |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | data_type         | No              | String           | Type of the data to be added to the OBS table. The options are as follows: Parquet, ORC, CSV, JSON, and Avro. |
   |                   |                 |                  |                                                                                                               |
   |                   |                 |                  | .. note::                                                                                                     |
   |                   |                 |                  |                                                                                                               |
   |                   |                 |                  |    This parameter is available only for OBS tables.                                                           |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | data_location     | Yes             | String           | Data storage location, which can be DLI or OBS.                                                               |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | last_access_time  | Yes             | Long             | Time when the table was last updated. The timestamp is expressed in milliseconds.                             |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | location          | No              | String           | Storage path on the OBS table.                                                                                |
   |                   |                 |                  |                                                                                                               |
   |                   |                 |                  | .. note::                                                                                                     |
   |                   |                 |                  |                                                                                                               |
   |                   |                 |                  |    This parameter is available only for OBS tables.                                                           |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | owner             | Yes             | String           | Table owner.                                                                                                  |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | table_name        | Yes             | String           | Name of a table.                                                                                              |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | table_size        | Yes             | Long             | Size of a DLI table. Set parameter to **0** for non-DLI tables. The unit is byte.                             |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | table_type        | Yes             | String           | Type of a table.                                                                                              |
   |                   |                 |                  |                                                                                                               |
   |                   |                 |                  | -  **EXTERNAL**: Indicates an OBS table.                                                                      |
   |                   |                 |                  | -  **MANAGED**: Indicates a DLI table.                                                                        |
   |                   |                 |                  | -  **VIEW**: Indicates a view.                                                                                |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | partition_columns | No              | Array of Strings | Partition field. This parameter is valid only for OBS partition tables.                                       |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | page-size         | No              | Integer          | Paging size. The minimum value is **1** and the maximum value is **100**.                                     |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+
   | current-page      | No              | Integer          | Current page number. The minimum value is **1**.                                                              |
   +-------------------+-----------------+------------------+---------------------------------------------------------------------------------------------------------------+

.. note::

   If **with-detail** is set to **false** in the URI, only values of tables-related parameters **data_location**, **table_name**, and **table_type** are returned.

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
     "is_success": true,
     "message": "",
     "table_count": 1,
     "tables": [
       { "create_time":1517364268000,
         "data_location":"OBS",
         "data_type":"csv",
         "last_access_time":1517364268000,
         "location":"obs://DLI/sqldata/data.txt",
         "owner":"test",
         "partition_columns": ["a0"],
         "table_name":"obs_t",
         "table_size":0,
         "table_type":"EXTERNAL"
       }
     ]
   }

Status Codes
------------

:ref:`Table 5 <dli_02_0105__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0105__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 5** Status codes

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
