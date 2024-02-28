:original_name: dli_08_0242.html

.. _dli_08_0242:

OBS Sink Stream
===============

Function
--------

Create a sink stream to export DLI data to OBS. DLI can export the job analysis results to OBS. OBS applies to various scenarios, such as big data analysis, cloud-native application program data, static website hosting, backup/active archive, and deep/cold archive.

OBS is an object-based storage service. It provides massive, secure, highly reliable, and low-cost data storage capabilities. For more information about OBS, see the *Object Storage Service Console Operation Guide*.

.. note::

   You are advised to use the :ref:`File System Sink Stream (Recommended) <dli_08_0267>`.

Prerequisites
-------------

Before data exporting, check the version of the OBS bucket. The OBS sink stream supports data exporting to an OBS bucket running OBS 3.0 or a later version.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
              type = "obs",
              region = "",
              encode = "",
              field_delimiter = "",
              row_delimiter = "",
              obs_dir = "",
              file_prefix = "",
              rolling_size = "",
              rolling_interval = "",
              quote = "",
              array_bracket = "",
              append = "",
              max_record_num_per_file = "",
              dump_interval = "",
              dis_notice_channel = ""
     )

Keywords
--------

.. table:: **Table 1** Keywords

   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory             | Description                                                                                                                                                                                                                                                                                                                                                |
   +=========================+=======================+============================================================================================================================================================================================================================================================================================================================================================+
   | type                    | Yes                   | Output channel type. **obs** indicates that data is exported to OBS.                                                                                                                                                                                                                                                                                       |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                  | Yes                   | Region to which OBS belongs.                                                                                                                                                                                                                                                                                                                               |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ak                      | No                    | Access Key ID (AK).                                                                                                                                                                                                                                                                                                                                        |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sk                      | No                    | Secret access key used together with the ID of the access key.                                                                                                                                                                                                                                                                                             |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                  | Yes                   | Encoding format. Currently, formats CSV, JSON, ORC, Avro, Avro-Merge, and Parquet are supported.                                                                                                                                                                                                                                                           |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | field_delimiter         | No                    | Separator used to separate every two attributes.                                                                                                                                                                                                                                                                                                           |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                            |
   |                         |                       | This parameter is mandatory only when the CSV encoding format is adopted. If this parameter is not specified, the default separator comma (,) is used.                                                                                                                                                                                                     |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | row_delimiter           | No                    | Row delimiter. This parameter does not need to be configured if the CSV or JSON encoding format is adopted.                                                                                                                                                                                                                                                |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | json_config             | No                    | If **encode** is set to **json**, you can set this parameter to specify the mapping between the JSON field and the stream definition field. An example of the format is as follows: field1=data_json.field1;field2=data_json.field2.                                                                                                                       |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | obs_dir                 | Yes                   | Directory for storing files. The directory is in the format of {Bucket name}/{Directory name}, for example, obs-a1/dir1/subdir. If **encode** is set to **csv** (**append** is **false**), **json** (**append** is **false**), **avro_merge**, or **parquet**, parameterization is supported.                                                              |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_prefix             | No                    | Prefix of the data export file name. The generated file is named in the format of file_prefix.\ *x*, for example, file_prefix.1 and file_prefix.2. If this parameter is not specified, the file prefix is **temp** by default.                                                                                                                             |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rolling_size            | No                    | Maximum size of a file.                                                                                                                                                                                                                                                                                                                                    |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                            |
   |                         |                       | .. note::                                                                                                                                                                                                                                                                                                                                                  |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                            |
   |                         |                       |    -  One or both of **rolling_size** and **rolling_interval** must be configured.                                                                                                                                                                                                                                                                         |
   |                         |                       |    -  When the size of a file exceeds the specified size, a new file is generated.                                                                                                                                                                                                                                                                         |
   |                         |                       |    -  The unit can be KB, MB, or GB. If no unit is specified, the byte unit is used.                                                                                                                                                                                                                                                                       |
   |                         |                       |    -  This parameter does not need to be configured if the ORC encoding format is adopted.                                                                                                                                                                                                                                                                 |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rolling_interval        | No                    | Time mode, in which data is saved to the corresponding directory.                                                                                                                                                                                                                                                                                          |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                            |
   |                         |                       | .. note::                                                                                                                                                                                                                                                                                                                                                  |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                            |
   |                         |                       |    -  One or both of **rolling_size** and **rolling_interval** must be configured.                                                                                                                                                                                                                                                                         |
   |                         |                       |    -  After this parameter is specified, data is written to the corresponding directories according to the output time.                                                                                                                                                                                                                                    |
   |                         |                       |    -  The parameter value can be in the format of yyyy/MM/dd/HH/mm, which is case sensitive. The minimum unit is minute. If this parameter is set to **yyyy/MM/dd/HH**, data is written to the directory that is generated at the hour time. For example, data generated at 2018-09-10 16:00 will be written to the **{obs_dir}/2018-09-10_16** directory. |
   |                         |                       |    -  If both **rolling_size** and **rolling_interval** are set, a new file is generated when the size of a single file exceeds the specified size in the directory corresponding to each time point.                                                                                                                                                      |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | quote                   | No                    | Modifier, which is added before and after each attribute only when the CSV encoding format is adopted. You are advised to use invisible characters, such as **u0007**, as the parameter value.                                                                                                                                                             |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | array_bracket           | No                    | Array bracket, which can be configured only when the CSV encoding format is adopted. The available options are **()**, **{}**, and **[]**. For example, if you set this parameter to **{}**, the array output format is {a1, a2}.                                                                                                                          |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | append                  | No                    | The value can be **true** or **false**. The default value is **true**.                                                                                                                                                                                                                                                                                     |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                            |
   |                         |                       | If OBS does not support the append mode and the encoding format is CSV or JSON, set this parameter to **false**. If **Append** is set to **false**, **max_record_num_per_file** and **dump_interval** must be set.                                                                                                                                         |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_record_num_per_file | No                    | Maximum number of records in a file. This parameter needs to be set if **encode** is **csv** (**append** is **false**), **json** (**append** is **false**), **orc**, **avro**, **avro_merge**, or **parquet**. If the maximum number of records has been reached, a new file is generated.                                                                 |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dump_interval           | No                    | Triggering period. This parameter needs to be configured when the ORC encoding format is adopted or notification to DIS is enabled.                                                                                                                                                                                                                        |
   |                         |                       |                                                                                                                                                                                                                                                                                                                                                            |
   |                         |                       | -  If the ORC encoding format is specified, this parameter indicates that files will be uploaded to OBS when the triggering period arrives even if the number of file records does not reach the maximum value.                                                                                                                                            |
   |                         |                       | -  In notification to DIS is enabled, this parameter specifies that a notification is sent to DIS every period to indicate that no more files will be generated in the directory.                                                                                                                                                                          |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dis_notice_channel      | No                    | DIS channel where DLI sends the record that contains the OBS directory DLI periodically sends the DIS channel a record, which contains the OBS directory, indicating that no more new files will be generated in the directory.                                                                                                                            |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encoded_data            | No                    | Data to be encoded. This parameter is set if **encode** is **json** (**append** is **false**), **avro_merge**, or **parquet**. The format is **${field_name}**, indicating that the stream field content is encoded as a complete record.                                                                                                                  |
   +-------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

If a configuration item can be specified through parameter configurations, one or more columns in the record can be used as part of the configuration item. For example, if the configuration item is set to **car_$ {car_brand}** and the value of **car_brand** in a record is **BMW**, the value of this configuration item is **car_BMW** in the record.

Example
-------

-  Export the **car_infos** data to the **obs-sink** bucket in OBS. The output directory is **car_infos**. The output file uses **greater_30** as the file name prefix. The maximum size of a single file is 100 MB. If the data size exceeds 100 MB, another new file is generated. The data is encoded in CSV format, the comma (,) is used as the attribute delimiter, and the line break is used as the line separator.

   ::

      CREATE SINK STREAM car_infos (
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_price INT,
        car_timestamp LONG
      )
        WITH (
          type = "obs",
          encode = "csv",
          region = "xxx",
          field_delimiter = ",",
          row_delimiter = "\n",
          obs_dir = "obs-sink/car_infos",
          file_prefix = "greater_30",
          rolling_size = "100m"
      );

-  Example of the ORC encoding format

   ::

      CREATE SINK STREAM car_infos (
        car_id STRING,
        car_owner STRING,
        car_brand STRING,
        car_price INT,
        car_timestamp LONG
      )
        WITH (
          type = "obs",
          region = "xxx",
          encode = "orc",
          obs_dir = "dli-append-2/obsorc",
          FILE_PREFIX = "es_info",
          max_record_num_per_file = "100000",
          dump_interval = "60"
      );

-  For details about the parquet encoding example, see the example in :ref:`File System Sink Stream (Recommended) <dli_08_0267>`.
