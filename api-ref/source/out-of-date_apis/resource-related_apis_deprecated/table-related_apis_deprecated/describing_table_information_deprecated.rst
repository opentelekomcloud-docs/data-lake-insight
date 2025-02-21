:original_name: dli_02_0033.html

.. _dli_02_0033:

Describing Table Information (Deprecated)
=========================================

Function
--------

This API is used to describe metadata information in the specified table.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   GET /v1.0/{project_id}/databases/{database_name}/tables/{table_name}

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of the database where the target table resides.                                                                                          |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | table_name    | Yes       | String | Name of the target table.                                                                                                                     |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 2** Response parameters

   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory       | Type             | Description                                                                                                                                                                   |
   +====================+=================+==================+===============================================================================================================================================================================+
   | is_success         | Yes             | Boolean          | Indicates whether the request is successfully executed. Value **true** indicates that the request is successfully executed.                                                   |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | message            | Yes             | String           | System prompt. If execution succeeds, the parameter setting may be left blank.                                                                                                |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | column_count       | Yes             | Integer          | Total number of columns in the table.                                                                                                                                         |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | columns            | Yes             | Array of objects | Column information, including the column name, type, and description. For details, see :ref:`Table 3 <dli_02_0033__table12769172353815>`.                                     |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_type         | Yes             | String           | Table type. The options are as follows:                                                                                                                                       |
   |                    |                 |                  |                                                                                                                                                                               |
   |                    |                 |                  | **MANAGED**: DLI table                                                                                                                                                        |
   |                    |                 |                  |                                                                                                                                                                               |
   |                    |                 |                  | **EXTERNAL**: OBS table                                                                                                                                                       |
   |                    |                 |                  |                                                                                                                                                                               |
   |                    |                 |                  | **VIEW**: view                                                                                                                                                                |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_type          | No              | String           | Data type, including **CSV**, **Parquet**, **ORC**, **JSON**, and **Avro**.                                                                                                   |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_location      | No              | String           | Path for storing data, which is an OBS path.                                                                                                                                  |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | storage_properties | No              | Array of objects | Storage attribute, which is in the format of **key/value** and includes parameters **delimiter**, **escape**, **quote**, **header**, **dateformat**, and **timestampformat**. |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment      | No              | String           | Table comment.                                                                                                                                                                |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | create_table_sql   | No              | String           | Statement used to create a table.                                                                                                                                             |
   +--------------------+-----------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0033__table12769172353815:

.. table:: **Table 3** **columns** parameters

   +---------------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter           | Mandatory | Type    | Description                                                                                                                                                                                                                          |
   +=====================+===========+=========+======================================================================================================================================================================================================================================+
   | column_name         | Yes       | String  | Column name.                                                                                                                                                                                                                         |
   +---------------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | description         | Yes       | String  | Description of a column.                                                                                                                                                                                                             |
   +---------------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | type                | Yes       | String  | Data type of a column.                                                                                                                                                                                                               |
   +---------------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | is_partition_column | Yes       | Boolean | Indicates whether the column is a partition column. The value **true** indicates that the column is a partition column, and the value **false** indicates that the column is not a partition column. The default value is **false**. |
   +---------------------+-----------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

-  **MANAGED** type table

   .. code-block::

      {
        "is_success": true,
        "message": "",
        "column_count": 3,
        "columns": [
          {
            "column_name": "id",
            "description": "",
            "type": "int",
            "is_partition_column": false
          },
          {
            "column_name": "name",
            "description": "",
            "type": "string",
            "is_partition_column": false
          },
          {
            "column_name": "level",
            "description": "",
            "type": "string",
            "is_partition_column": true
          }
        ],
          "table_type":"MANAGED"
      }

-  **EXTERNAL** type table

   .. code-block::

      {
          "is_success": true,
          "message": "",
          "column_count": 2,
          "columns": [
              {
                  "type": "string",
                  "description": "",
                  "column_name": "col2",
                  "is_partition_column": false
              },
              {
                  "type": "string",
                  "description": "",
                  "column_name": "col1",
                  "is_partition_column": true
              }
          ],
          "table_type": "EXTERNAL",
          "data_type": "parquet",
          "data_location": "obs://obs-wangtao/savepoint/savepoint-d95437-039668840fff/_metadata",
          "storage_properties": [
              {
                  "key": "timestampformat",
                  "value": "yyyy-MM-dd HH:mm:ss"
              },
              {
                  "key": "quote",
                  "value": "\""
              },
              {
                  "key": "dateformat",
                  "value": "yyyy-MM-dd"
              },
              {
                  "key": "escape",
                  "value": "\\"
              },
              {
                  "key": "header",
                  "value": "false"
              },
              {
                  "key": "delimiter",
                  "value": ","
              }
          ],
          "table_comment": "",
           "create_table_sql": "CREATE TABLE `default`.`wan_test` (`col2` STRING, `col1` STRING)\nUSING parquet\nOPTIONS (\n  `timestampformat` 'yyyy-MM-dd HH:mm:ss',\n  `quote` '\"',\n  `dateformat` 'yyyy-MM-dd',\n  `escape` '\\\\',\n  `header` 'false',\n  `delimiter` ','\n)\nPARTITIONED BY (col1)\nCOMMENT ''\nLOCATION 'obs://obs-wangtao/savepoint/savepoint-d95437-039668840fff/_metadata'\nTBLPROPERTIES (\n  'hive.serialization.extend.nesting.levels' = 'true'\n)\n"
         }

-  **VIEW** type table

   .. code-block::

      {
        "is_success": true,
        "message": "",
        "column_count": 3,
        "columns": [
          {
            "column_name": "id",
            "description": "",
            "type": "int",
            "is_partition_column": false
          },
          {
            "column_name": "name",
            "description": "",
            "type": "string",
            "is_partition_column": false
          },
          {
            "column_name": "level",
            "description": "",
            "type": "string",
            "is_partition_column": true
          }
        ],
        "table_type":"VIEW",
        "create_table_sql": "CREATE VIEW `default`.`view1`(id, name) AS\nselect * from a_gff.testtable\n"
      }

Status Codes
------------

:ref:`Table 4 <dli_02_0033__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0033__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 4** Status codes

   =========== ============================
   Status Code Description
   =========== ============================
   200         The operation is successful.
   400         Request error.
   500         Internal service error.
   =========== ============================

Error Codes
-----------

If an error occurs when this API is invoked, the system does not return the result similar to the preceding example, but returns the error code and error information. For details, see :ref:`Error Codes <dli_02_0056>`.
