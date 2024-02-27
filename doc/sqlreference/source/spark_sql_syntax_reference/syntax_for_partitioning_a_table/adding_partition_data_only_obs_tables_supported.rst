:original_name: dli_08_0081.html

.. _dli_08_0081:

Adding Partition Data (Only OBS Tables Supported)
=================================================

Function
--------

After an OBS partitioned table is created, no partition information is generated for the table. Partition information is generated only after you:

-  Insert data to the OBS partitioned table. After the data is inserted successfully, the partition metadata can be queried, for example, by partition columns.
-  Copy the partition directory and data into the OBS path of the partitioned table, and run the partition adding statements described in this section to generate partition metadata. Then you can perform operations such as table query by partition columns.

The following describes how to use the **ALTER TABLE** statement to add a partition.

Syntax
------

::

   ALTER TABLE table_name ADD [IF NOT EXISTS]
     PARTITION partition_specs1
     [LOCATION 'obs_path1']
     PARTITION partition_specs2
     [LOCATION 'obs_path2'];

Keywords
--------

-  IF NOT EXISTS: prevents errors when partitions are repeatedly added.
-  PARTITION: specifies a partition.
-  LOCATION: specifies the partition path.

Parameters
----------

.. table:: **Table 1** Parameters

   =============== ================
   Parameter       Description
   =============== ================
   table_name      Table name
   partition_specs Partition fields
   obs_path        OBS path
   =============== ================

Precautions
-----------

-  When you add a partition to a table, the table and the partition column (specified by PARTITIONED BY during table creation) must exist, and the partition to be added cannot be added repeatedly. Otherwise, an error is reported. You can use **IF NOT EXISTS** to avoid errors if the partition does not exist.

-  If tables are partitioned by multiple fields, you need to specify all partitioning fields in any sequence when adding partitions.

-  By default, parameters in **partition_specs** contain parentheses (). For example: **PARTITION (dt='2009-09-09',city='xxx')**.

-  If you need to specify an OBS path when adding a partition, the OBS path must exist. Otherwise, an error occurs.

-  To add multiple partitions, you need to use spaces to separate each set of **LOCATION 'obs_path'** in the **PARTITION partition_specs**. The following is an example:

   **PARTITION partition_specs LOCATION 'obs_path' PARTITION partition_specs LOCATION 'obs_path'**

-  If the path specified in the new partition contains subdirectories (or nested subdirectories), all file types and content in the subdirectories are considered partition records.

   Ensure that all file types and file content in the partition directory are the same as those in the table. Otherwise, an error is reported.

   You can set **multiLevelDirEnable** to **true** in the **OPTIONS** statement to query the content in the subdirectory. The default value is **false** (Note that this configuration item is a table attribute, exercise caution when performing this operation. Hive tables do not support this configuration item.)

Example
-------

-  The following example shows you how to add partition data when the OBS table is partitioned by a single column.

   #. Use the DataSource syntax to create an OBS table, and partition the table by column **external_data**. The partition data is stored in **obs://bucketName/datapath**.

      .. code-block::

         create table testobstable(id varchar(128), external_data varchar(16)) using JSON OPTIONS (path 'obs://bucketName/datapath') PARTITIONED by (external_data);

   #. Copy the partition directory to **obs://bucketName/datapath**. In this example, copy all files in the partition column **external_data=22** to **obs://bucketName/datapath**.

   #. Run the following command to add partition data:

      .. code-block::

         ALTER TABLE testobstable ADD
           PARTITION (external_data='22')
           LOCATION 'obs://bucketName/datapath/external_data=22';

   #. After the partition data is added successfully, you can perform operations such as data query based on the partition column.

      .. code-block::

         select * from testobstable where external_data='22';

-  The following example shows you how to add partition data when the OBS table is partitioned by multiple columns.

   #. Use the DataSource syntax to create an OBS table, and partition the table by columns **external_data** and **dt**. The partition data is stored in **obs://bucketName/datapath**.

      ::

         create table testobstable(
           id varchar(128),
           external_data varchar(16),
           dt varchar(16)
         ) using JSON OPTIONS (path 'obs://bucketName/datapath') PARTITIONED by (external_data, dt);

   #. Copy the partition directories to **obs://bucketName/datapath**. In this example, copy files in **external_data=22** and its subdirectory **dt=2021-07-27** to **obs://bucketName/datapath**.

   #. Run the following command to add partition data:

      ::

         ALTER TABLE
           testobstable
         ADD
           PARTITION (external_data = '22', dt = '2021-07-27') LOCATION 'obs://bucketName/datapath/external_data=22/dt=2021-07-27';

   #. After the partition data is added successfully, you can perform operations such as data query based on the partition columns.

      ::

         select * from testobstable where external_data = '22';
         select * from testobstable where external_data = '22' and dt='2021-07-27';
