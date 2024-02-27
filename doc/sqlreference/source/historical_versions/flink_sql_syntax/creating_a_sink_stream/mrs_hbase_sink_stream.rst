:original_name: dli_08_0255.html

.. _dli_08_0255:

MRS HBase Sink Stream
=====================

Function
--------

DLI exports the output data of the Flink job to HBase of MRS.

Prerequisites
-------------

-  An MRS cluster has been created by using your account. DLI can interconnect with HBase clusters with Kerberos enabled.

-  In this scenario, jobs must run on the dedicated queue of DLI. Ensure that the dedicated queue of DLI has been created.

-  Ensure that a datasource connection has been set up between the DLI dedicated queue and the MRS cluster, and security group rules have been configured based on the site requirements.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

-  **If you use MRS HBase, ensure that you have added IP addresses of all hosts in the MRS cluster for the enhanced datasource connection.**

   For details about how to add an IP-domain mapping, see **Datasource Connections >Enhanced Datasource Connections > Modifying the Host Information** in the *Data Lake Insight User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "mrs_hbase",
       region = "",
       cluster_address = "",
       table_name = "",
       table_columns = "",
       illegal_data_table = "",
       batch_insert_data_num = "",
       action = ""
   )

Keywords
--------

.. table:: **Table 1** Keywords

   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                          |
   +=======================+=======================+======================================================================================================================================================================================================================================================================================================+
   | type                  | Yes                   | Output channel type. **mrs_hbase** indicates that data is exported to HBase of MRS.                                                                                                                                                                                                                  |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes                   | Region where MRS resides.                                                                                                                                                                                                                                                                            |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_address       | Yes                   | ZooKeeper address of the cluster to which the data table to be inserted belongs. The format is **ip1,ip2:port**.                                                                                                                                                                                     |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes                   | Name of the table where data is to be inserted.                                                                                                                                                                                                                                                      |
   |                       |                       |                                                                                                                                                                                                                                                                                                      |
   |                       |                       | It can be specified through parameter configurations. For example, if you want one or more certain columns as part of the table name, use **car_pass_inspect_with_age_${car_age}**, where **car_age** is the column name.                                                                            |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_columns         | Yes                   | Columns to be inserted. The format is **rowKey, f1:c1, f1:c2, f2:c1**, where **rowKey** must be specified. If you do not want to add a column (for example, the third column) to the database, set this parameter to **rowKey,f1:c1,,f2:c1**.                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | illegal_data_table    | No                    | If this parameter is specified, abnormal data (for example, **rowKey** does not exist) will be written into the table. If not specified, abnormal data will be discarded. The **rowKey** value is **taskNo\_**\ *Timestamp* followed by six random digits, and the schema is info:data, info:reason. |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | batch_insert_data_num | No                    | Number of data records to be written in batches at a time. The value must be a positive integer. The upper limit is **1000**. The default value is **10**.                                                                                                                                           |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | action                | No                    | Whether data is added or deleted. Available options include **add** and **delete**. The default value is **add**.                                                                                                                                                                                    |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | krb_auth              | No                    | Authentication name for creating a datasource connection authentication. This parameter is mandatory when Kerberos authentication is enabled. Set this parameter to the corresponding cross-source authentication name.                                                                              |
   |                       |                       |                                                                                                                                                                                                                                                                                                      |
   |                       |                       | .. note::                                                                                                                                                                                                                                                                                            |
   |                       |                       |                                                                                                                                                                                                                                                                                                      |
   |                       |                       |    Ensure that the **/etc/hosts** information of the master node in the MRS cluster is added to the host file of the DLI queue.                                                                                                                                                                      |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

None

Example
-------

Output data to HBase of MRS.

::

   CREATE SINK STREAM qualified_cars (
     car_id STRING,
     car_owner STRING,
     car_age INT,
     average_speed INT,
     total_miles INT
   )
     WITH (
       type = "mrs_hbase",
       region = "xxx",
       cluster_address = "192.16.0.88,192.87.3.88:2181",
       table_name = "car_pass_inspect_with_age_${car_age}",
       table_columns = "rowKey,info:owner,,car:speed,car:miles",
       illegal_data_table = "illegal_data",
       batch_insert_data_num = "20",
       action = "add",
       krb_auth = "KRB_AUTH_NAME"
     );
