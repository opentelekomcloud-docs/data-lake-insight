:original_name: dli_08_0248.html

.. _dli_08_0248:

DWS Sink Stream (OBS-based Dumping)
===================================

Function
--------

Create a sink stream to export Flink job data to DWS through OBS-based dumping, specifically, output Flink job data to OBS and then import data from OBS to DWS. For details about how to import OBS data to DWS, see **Concurrently Importing Data from OBS** in the Data Warehouse Service Development Guide\ *Data Warehouse Service Development Guide*.

DWS is an online data processing database based on the cloud infrastructure and platform and helps you mine and analyze massive sets of data. For more information about DWS, see the .

Precautions
-----------

-  OBS-based dumping supports intermediate files of the following two types:

   -  ORC: The ORC format does not support array data type. If the ORC format is used, create a foreign server in DWS. For details, see **Creating a Foreign Server** in the .
   -  CSV: By default, the line break is used as the record separator. If the line break is contained in the attribute content, you are advised to configure quote. For details, see :ref:`Table 1 <dli_08_0248__en-us_topic_0125566718_table1648420306385>`.

-  If the target table does not exist, a table is automatically created. DLI data of the SQL type does not support **text**. If a long text exists, you are advised to create a table in the database.
-  When **encode** uses the ORC format to create a DWS table, if the field attribute of the SQL stream is defined as the **String** type, the field attribute of the DWS table cannot use the **varchar** type. Instead, a specific text type must be used. If the SQL stream field attribute is defined as the **Integer** type, the DWS table field must use the **Integer** type.

Prerequisites
-------------

-  Ensure that OBS buckets and folders have been created.

   For details about how to create an OBS bucket, see **Creating a Bucket** in the *Object Storage Service User Guide*.

   For details about how to create a folder, see **Creating a Folder** in the *Object Storage Service User Guide*.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with DWS clusters. You can also set the security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
             type = "dws",
             region = "",
             ak = "",
             sk = "",
             encode = "",
             field_delimiter = "",
             quote = "",
             db_obs_server = "",
             obs_dir = "",
             username = "",
             password =  "",
             db_url = "",
             table_name = "",
             max_record_num_per_file = "",
             dump_interval = ""
     );

Keywords
--------

.. _dli_08_0248__en-us_topic_0125566718_table1648420306385:

.. table:: **Table 1** Keywords

   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory             | Description                                                                                                                                                                                                                  |
   +=========================+=======================+==============================================================================================================================================================================================================================+
   | type                    | Yes                   | Output channel type. **dws** indicates that data is exported to DWS.                                                                                                                                                         |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                  | Yes                   | Region where DWS is located.                                                                                                                                                                                                 |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ak                      | Yes                   | Access Key ID (AK).                                                                                                                                                                                                          |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sk                      | Yes                   | Secret access key used together with the ID of the AK.                                                                                                                                                                       |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                  | Yes                   | Encoding format. Currently, CSV and ORC are supported.                                                                                                                                                                       |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | field_delimiter         | No                    | Separator used to separate every two attributes. This parameter needs to be configured if the CSV encoding mode is used. It is recommended that you use invisible characters as separators, for example, **\\u0006\\u0002**. |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote                   | No                    | Single byte. It is recommended that invisible characters be used, for example, **u0007**.                                                                                                                                    |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_obs_server           | No                    | Foreign server (for example, **obs_server**) that has been created in the database.                                                                                                                                          |
   |                         |                       |                                                                                                                                                                                                                              |
   |                         |                       | You need to specify this parameter if the ORC encoding mode is adopted.                                                                                                                                                      |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | obs_dir                 | Yes                   | Directory for storing intermediate files. The directory is in the format of {Bucket name}/{Directory name}, for example, obs-a1/dir1/subdir.                                                                                 |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                | Yes                   | Username for connecting to a database.                                                                                                                                                                                       |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                | Yes                   | Password for connecting to a database.                                                                                                                                                                                       |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_url                  | Yes                   | Database connection address. The format is /ip:port/database, for example, **192.168.1.21:8000/test1**.                                                                                                                      |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name              | Yes                   | Data table name. If no table is available, a table is automatically created.                                                                                                                                                 |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_record_num_per_file | Yes                   | Maximum number of records that can be stored in a file. If the number of records in a file is less than the maximum value, the file will be dumped to OBS after one dumping period.                                          |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dump_interval           | Yes                   | Dumping period. The unit is second.                                                                                                                                                                                          |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | delete_obs_temp_file    | No                    | Whether to delete temporary files on OBS. The default value is **true**. If this parameter is set to **false**, files on OBS will not be deleted. You need to manually clear the files.                                      |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_dump_file_num       | No                    | Maximum number of files that can be dumped at a time. If the number of files to be dumped is less than the maximum value, the files will be dumped to OBS after one dumping period.                                          |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

-  Dump files in CSV format.

   ::

      CREATE SINK STREAM car_infos (
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_price INT,
        car_timestamp LONG
      )
        WITH (
          type = "dws",
          region = "xxx",
          ak = "",
          sk = "",
          encode = "csv",
          field_delimiter = "\u0006\u0006\u0002",
          quote = "\u0007",
          obs_dir = "dli-append-2/dws",
          username = "",
          password =  "",
          db_url = "192.168.1.12:8000/test1",
          table_name = "table1",
          max_record_num_per_file = "100",
          dump_interval = "10"
        );

-  Dump files in ORC format.

   ::

      CREATE SINK STREAM car_infos (
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_price INT,
        car_timestamp LONG
      )
        WITH (
          type = "dws",
          region = "xxx",
          ak = "",
          sk = "",
          encode = "orc",
          db_obs_server = "obs_server",
          obs_dir = "dli-append-2/dws",
          username = "",
          password =  "",
          db_url = "192.168.1.12:8000/test1",
          table_name = "table1",
          max_record_num_per_file = "100",
          dump_interval = "10"
        );
