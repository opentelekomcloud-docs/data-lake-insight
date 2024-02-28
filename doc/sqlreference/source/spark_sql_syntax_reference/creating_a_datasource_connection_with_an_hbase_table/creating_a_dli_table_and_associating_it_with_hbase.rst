:original_name: dli_08_0119.html

.. _dli_08_0119:

Creating a DLI Table and Associating It with HBase
==================================================

Function
--------

This statement is used to create a DLI table and associate it with an existing HBase table.

.. note::

   In Spark cross-source development scenarios, there is a risk of password leakage if datasource authentication information is directly configured. You are advised to use the datasource authentication provided by DLI.

Prerequisites
-------------

-  Before creating a DLI table and associating it with HBase, you need to create a datasource connection. For details about operations on the management console, see

-  Ensure that the **/etc/hosts** information of the master node in the MRS cluster is added to the host file of the DLI queue.

   For details about how to add an IP-domain mapping, see **Enhanced Datasource Connection** in the *Data Lake Insight User Guide*.

-  The syntax is not supported for security clusters.

Syntax
------

-  Single row key

   ::

      CREATE TABLE [IF NOT EXISTS] TABLE_NAME (
        ATTR1 TYPE,
        ATTR2 TYPE,
        ATTR3 TYPE)
        USING [CLOUDTABLE | HBASE] OPTIONS (
        'ZKHost'='xx',
        'TableName'='TABLE_IN_HBASE',
        'RowKey'='ATTR1',
        'Cols'='ATTR2:CF1.C1, ATTR3:CF1.C2');

-  Combined row key

   ::

      CREATE TABLE [IF NOT EXISTS] TABLE_NAME (
        ATTR1 String,
        ATTR2 String,
        ATTR3 TYPE)
        USING [CLOUDTABLE | HBASE] OPTIONS (
        'ZKHost'='xx',
        'TableName'='TABLE_IN_HBASE',
        'RowKey'='ATTR1:2, ATTR2:10',
        'Cols'='ATTR2:CF1.C1, ATTR3:CF1.C2'

Keywords
--------

.. table:: **Table 1** CREATE TABLE keywords

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                        |
   +===================================+====================================================================================================================================================================================================================================================================================================================================================================+
   | USING [CLOUDTABLE \| HBASE]       | Specify the HBase datasource to CLOUDTABLE or HBASE. The value is case insensitive.                                                                                                                                                                                                                                                                                |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ZKHost                            | ZooKeeper IP address of the HBase cluster.                                                                                                                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                    |
   |                                   | Before obtaining the ZooKeeper IP address, you need to create a datasource connection first..                                                                                                                                                                                                                                                                      |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                    |
   |                                   | -  Access the CloudTable cluster and enter the ZooKeeper IP address (internal network).                                                                                                                                                                                                                                                                            |
   |                                   | -  To access the MRS cluster, enter the IP address of the node where the ZooKeeper is located and the external port number of the ZooKeeper. The format is **ZK_IP1:ZK_PORT1,ZK_IP2:ZK_PORT2**.                                                                                                                                                                    |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TableName                         | Specifies the name of a table that has been created in the HBase cluster.                                                                                                                                                                                                                                                                                          |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | RowKey                            | Specifies the row key field of the table connected to DLI. The single and composite row keys are supported. A single row key can be of the numeric or string type. The length does not need to be specified. The composite row key supports only fixed-length data of the string type. The format is **attribute name 1:Length, attribute name 2:length**.         |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Cols                              | Provides mappings between fields in the DLI table and columns in the HBase table. The mappings are separated by commas (,). In a mapping, the field in the DLI table is located before the colon (:) and information about the HBase table follows the colon (:). In the HBase table information, the column family and column name are separated using a dot (.). |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  If the to-be-created table exists, an error is reported. To avoid such error, add **IF NOT EXISTS** in this statement.
-  All parameters in OPTIONS are mandatory. Parameter names are case-insensitive, while parameter values are case-sensitive.
-  In OPTIONS, spaces are not allowed before or after the value in the quotation marks because spaces are also considered as a part of the value.
-  Descriptions of table names and column names support only string constants.
-  When creating a table, specify the column name and the corresponding data types. Currently, supported data types include Boolean, short, int, long, float, double, and string.
-  The value of **row key** (for example, ATTR1) cannot be null, and its length must be greater than 0 and less than or equal to 32767.
-  The total number of fields in **Cols** and **row key** must be the same as that in the DLI table. Specifically, all fields in the table are mapped to **Cols** and **row key** without sequence requirements specified.
-  The combined row key only supports data of the string type. If the combined row key is used, the length must follow each attribute name. If only one field is specified as the row key, the field type can be any supported data type and you do not need to specify the length.
-  If the combined row key is used:

   -  When the row key is inserted, if the actual attribute length is shorter than the specified length when the attribute is used as the row key, add **\\0** after the attribute. If it is longer, the attribute will be truncated when it is inserted into HBase.
   -  When reading the **row key** field in HBase, if the actual data length of an attribute is shorter than that specified when the attribute is used as the **row key**, an error message (**OutofBoundException**) is reported. If it is longer, the attribute will be truncated during data reading.

Example
-------

::

   CREATE TABLE test_hbase(
   ATTR1 int,
   ATTR2 int,
   ATTR3 string)
   using hbase OPTIONS (
   'ZKHost'='to-hbase-1174405101-CE1bDm5B.datasource.com:2181',
   'TableName'='HBASE_TABLE',
   'RowKey'='ATTR1',
   'Cols'='ATTR2:CF1.C1, ATTR3:CF1.C2');
