:original_name: dli_08_0267.html

.. _dli_08_0267:

File System Sink Stream (Recommended)
=====================================

Function
--------

You can create a sink stream to export data to a file system such as HDFS or OBS. After the data is generated, a non-DLI table can be created directly according to the generated directory. The table can be processed through DLI SQL, and the output data directory can be stored in partitioned tables. It is applicable to scenarios such as data dumping, big data analysis, data backup, and active, deep, or cold archiving.

OBS is an object-based storage service. It provides massive, secure, highly reliable, and low-cost data storage capabilities.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     [PARTITIONED BY (attr_name (',' attr_name)*]
     WITH (
       type = "filesystem",
       file.path = "obs://bucket/xx",
       encode = "parquet",
       ak = "",
       sk = ""
     );

Keywords
--------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                                                                       |
   +=======================+=======================+===================================================================================================================================================================================================================================================================================================================================================+
   | type                  | Yes                   | Output stream type. If **type** is set to **filesystem**, data is exported to the file system.                                                                                                                                                                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file.path             | Yes                   | Output directory in the form: **schema://file.path**.                                                                                                                                                                                                                                                                                             |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                       | Currently, Schema supports only OBS and HDFS.                                                                                                                                                                                                                                                                                                     |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                       | -  If **schema** is set to **obs**, data is stored to OBS.                                                                                                                                                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                       | -  If **schema** is set to **hdfs**, data is exported to HDFS. A proxy user needs to be configured for HDFS. For details, see :ref:`HDFS Proxy User Configuration <dli_08_0267__section11762174112291>`.                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                       |    Example: **hdfs://node-master1sYAx:9820/user/car_infos**, where **node-master1sYAx:9820** is the name of the node where the NameNode is located.                                                                                                                                                                                               |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | encode                | Yes                   | Output data encoding format. Currently, only the **parquet** and **csv** formats are supported.                                                                                                                                                                                                                                                   |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                       | -  When **schema** is set to **obs**, the encoding format of the output data can only be **parquet**.                                                                                                                                                                                                                                             |
   |                       |                       | -  When **schema** is set to **hdfs**, the output data can be encoded in **Parquet** or **CSV** format.                                                                                                                                                                                                                                           |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ak                    | No                    | Access key. This parameter is mandatory when data is exported to OBS. Global variables can be used to mask the access key used for OBS authentication.                                                                                                                                                                                            |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sk                    | No                    | Secret access key. This parameter is mandatory when data is exported to OBS. Secret key for accessing OBS authentication. Global variables can be used to mask sensitive information.                                                                                                                                                             |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | krb_auth              | No                    | Authentication name for creating a datasource connection authentication. This parameter is mandatory when Kerberos authentication is enabled. If Kerberos authentication is not enabled for the created MRS cluster, ensure that the **/etc/hosts** information of the master node in the MRS cluster is added to the host file of the DLI queue. |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | field_delimiter       | No                    | Separator used to separate every two attributes.                                                                                                                                                                                                                                                                                                  |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                                   |
   |                       |                       | This parameter needs to be configured if the CSV encoding format is adopted. It can be user-defined, for example, a comma (**,**).                                                                                                                                                                                                                |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  To ensure job consistency, enable checkpointing if the Flink job uses the file system output stream.

-  To avoid data loss or data coverage, you need to enable automatic or manual restart upon job exceptions. Enable the **Restore Job from Checkpoint**.

-  Set the checkpoint interval after weighing between real-time output file, file size, and recovery time, such as 10 minutes.

-  Two modes are supported.

   -  **At least once**: Events are processed at least once.
   -  **Exactly once**: Events are processed only once.

-  When you use sink streams of a file system to write data into OBS, do not use multiple jobs for the same directory.

   -  The default behavior of an OBS bucket is overwriting, which may cause data loss.
   -  The default behavior of the OBS parallel file system bucket is appending, which may cause data confusion.

   You should carefully select the OBS bucket because of the preceding behavior differences. Data exceptions may occur after abnormal job restart.

.. _dli_08_0267__section11762174112291:

HDFS Proxy User Configuration
-----------------------------

#. Log in to the MRS management page.

#. Select the HDFS NameNode configuration of MRS and add configuration parameters in the **Customization** area.

   In the preceding information, **myname** in the **core-site** values **hadoop.proxyuser.myname.hosts** and **hadoop.proxyuser.myname.groups** is the name of the krb authentication user.

   .. note::

      Ensure that the permission on the HDFS data write path is **777**.

#. After the configuration is complete, click **Save**.

Example
-------

-  Example 1:

   The following example dumps the **car_info** data to OBS, with the **buyday** field as the partition field and **parquet** as the encoding format.

   ::

      create sink stream car_infos (
        carId string,
        carOwner string,
        average_speed double,
        buyday string
        ) partitioned by (buyday)
        with (
          type = "filesystem",
          file.path = "obs://obs-sink/car_infos",
          encode = "parquet",
          ak = "{{myAk}}",
          sk = "{{mySk}}"
      );

   The data is ultimately stored in OBS. Directory: **obs://obs-sink/car_infos/buyday=xx/part-x-x**.

   After the data is generated, the OBS partitioned table can be established for subsequent batch processing through the following SQL statements:

   #. Create an OBS partitioned table.

      ::

         create table car_infos (
           carId string,
           carOwner string,
           average_speed double
         )
           partitioned by (buyday string)
           stored as parquet
           location 'obs://obs-sink/car_infos';

   #. Restore partition information from the associated OBS path.

      ::

         alter table car_infos recover partitions;

-  Example 2:

   The following example dumps the **car_info** data to HDFS, with the **buyday** field as the partition field and **csv** as the encoding format.

   ::

      create sink stream car_infos (
        carId string,
        carOwner string,
        average_speed double,
        buyday string
        ) partitioned by (buyday)
        with (
          type = "filesystem",
          file.path = "hdfs://node-master1sYAx:9820/user/car_infos",
          encode = "csv",
          field_delimiter = ","
      );

   The data is ultimately stored in HDFS. Directory: **/user/car_infos/buyday=xx/part-x-x**.
