:original_name: dli_02_0250.html

.. _dli_02_0250:

Listing Partitions (Deprecated)
===============================

Function
--------

This API is used to list partitions.

.. note::

   This API has been deprecated and is not recommended.

URI
---

-  URI format

   GET /v1.0/{project_id}/databases/{database_name}/tables/{table_name}/partitions

-  Parameter description

   .. table:: **Table 1** URI parameters

      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter     | Mandatory | Type   | Description                                                                                                                                   |
      +===============+===========+========+===============================================================================================================================================+
      | project_id    | Yes       | String | Project ID, which is used for resource isolation. For details about how to obtain its value, see :ref:`Obtaining a Project ID <dli_02_0183>`. |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | database_name | Yes       | String | Name of a database.                                                                                                                           |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+
      | table_name    | Yes       | String | Name of a table.                                                                                                                              |
      +---------------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------+

   .. table:: **Table 2** query parameter description

      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                                                                              |
      +=================+=================+=================+==========================================================================================================================================================================================================================================================================================================================================+
      | limit           | No              | Integer         | Number of returned records displayed on each page. The default value is **100**.                                                                                                                                                                                                                                                         |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | offset          | No              | Integer         | Offset.                                                                                                                                                                                                                                                                                                                                  |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | filter          | No              | String          | Filtering condition. Currently, only the **=** condition is supported. For example, **name=name1** indicates that the data whose name is **name1** in the partition is filtered. **name** indicates the name of the partition column, and **name1** indicates the value of the partition column. The key and value are case insensitive. |
      |                 |                 |                 |                                                                                                                                                                                                                                                                                                                                          |
      |                 |                 |                 | Example: GET /v1.0/{*project_id*}/databases/{*database_name*}/tables/{*table_name*}/partitions?part=part2                                                                                                                                                                                                                                |
      +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Request
-------

None

Response
--------

.. table:: **Table 3** Response parameters

   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Mandatory | Type    | Description                                                                                                       |
   +============+===========+=========+===================================================================================================================+
   | is_success | No        | Boolean | Whether the request is successfully executed. Value **true** indicates that the request is successfully executed. |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | message    | No        | String  | System prompt. If execution succeeds, the parameter setting may be left blank.                                    |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+
   | partitions | No        | Object  | Partition information. For details, see :ref:`Table 4 <dli_02_0250__table10945172033612>`.                        |
   +------------+-----------+---------+-------------------------------------------------------------------------------------------------------------------+

.. _dli_02_0250__table10945172033612:

.. table:: **Table 4** partitions parameter description

   +-----------------+-----------+------------------+---------------------------------------------------------------------------------------+
   | Parameter       | Mandatory | Type             | Description                                                                           |
   +=================+===========+==================+=======================================================================================+
   | total_count     | Yes       | Long             | Total number of partitions.                                                           |
   +-----------------+-----------+------------------+---------------------------------------------------------------------------------------+
   | partition_infos | Yes       | Array of objects | List of partitions. For details, see :ref:`Table 5 <dli_02_0250__table118615164215>`. |
   +-----------------+-----------+------------------+---------------------------------------------------------------------------------------+

.. _dli_02_0250__table118615164215:

.. table:: **Table 5** partition_infos parameter description

   +------------------+-----------+------------------+------------------------------------------------------------+
   | Parameter        | Mandatory | Type             | Description                                                |
   +==================+===========+==================+============================================================+
   | partition_name   | Yes       | String           | Partition name.                                            |
   +------------------+-----------+------------------+------------------------------------------------------------+
   | create_time      | Yes       | Long             | Time when a partition is created.                          |
   +------------------+-----------+------------------+------------------------------------------------------------+
   | last_access_time | Yes       | Long             | Last update time.                                          |
   +------------------+-----------+------------------+------------------------------------------------------------+
   | locations        | No        | Array of Strings | Path. This parameter is displayed only for non-DLI tables. |
   +------------------+-----------+------------------+------------------------------------------------------------+
   | last_ddl_time    | No        | Long             | Execution time of the last DDL statement, in seconds.      |
   +------------------+-----------+------------------+------------------------------------------------------------+
   | num_rows         | No        | Long             | Total rows in the partition.                               |
   +------------------+-----------+------------------+------------------------------------------------------------+
   | num_files        | No        | Long             | Number of files in a partition.                            |
   +------------------+-----------+------------------+------------------------------------------------------------+
   | total_size       | No        | Long             | Total size of data in the partition, in bytes.             |
   +------------------+-----------+------------------+------------------------------------------------------------+

Example Request
---------------

None

Example Response
----------------

.. code-block::

   {
       "is_success": true,
       "message": "list partitions succeed",
       "partitions": {
           "total_count": 5,
           "partition_infos": [
               {
                   "partition_name": "name=test",
                   "create_time": 1579520179000,
                   "last_access_time": 1579520179000,
                   "locations": [
                       "obs://test/partition"
                   ]
               },
               {
                   "partition_name": "name=test1",
                   "create_time": 1579521406000,
                   "last_access_time": 1579521406000,
                   "locations": [
                       "obs://test/partition"
                   ]
               },
               {
                   "partition_name": "name=test2",
                   "create_time": 1579521884000,
                   "last_access_time": 1579521884000,
                   "locations": [
                       "obs://test/partition"
                   ]
               },
               {
                   "partition_name": "name=test3",
                   "create_time": 1579522085000,
                   "last_access_time": 1579522085000,
                   "locations": [
                       "obs://test/partition"
                   ]
               },
               {
                   "partition_name": "name=name1/age=age1",
                   "create_time": 1581409182000,
                   "last_access_time": 1581409182000,
                   "locations": [
                       "obs://test/0117"
                   ],
                   "last_ddl_time": 1581409182,
                   "total_size": 2130,
                   "num_rows": -1,
                   "num_files": 2
               }
           ]
       }
   }

Status Codes
------------

:ref:`Table 6 <dli_02_0250__tb12870f1c5f24b27abd55ca24264af36>` describes the status code.

.. _dli_02_0250__tb12870f1c5f24b27abd55ca24264af36:

.. table:: **Table 6** Status codes

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
