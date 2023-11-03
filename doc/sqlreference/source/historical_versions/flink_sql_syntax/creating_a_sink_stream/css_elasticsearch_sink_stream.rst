:original_name: dli_08_0252.html

.. _dli_08_0252:

CSS Elasticsearch Sink Stream
=============================

Function
--------

DLI exports Flink job output data to Elasticsearch of Cloud Search Service (CSS). Elasticsearch is a popular enterprise-class Lucene-powered search server and provides the distributed multi-user capabilities. It delivers multiple functions, including full-text retrieval, structured search, analytics, aggregation, and highlighting. With Elasticsearch, you can achieve stable, reliable, real-time search. Elasticsearch applies to diversified scenarios, such as log analysis and site search.

CSS is a fully managed, distributed search service. It is fully compatible with open-source Elasticsearch and provides DLI with structured and unstructured data search, statistics, and report capabilities.

For more information about CSS, see the *Cloud Search Service User Guide*.

.. note::

   If the security mode is enabled when you create a CSS cluster, it cannot be undone.

Prerequisites
-------------

-  Ensure that you have created a cluster on CSS using your account. For details about how to create a cluster on CSS, see **Creating a Cluster** in the *Cloud Search Service User Guide*.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with CSS. You can also set the security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "es",
       region = "",
       cluster_address = "",
       es_index = "",
       es_type= "",
       es_fields= "",
       batch_insert_data_num= ""
     );

Keyword
-------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                                              |
   +=======================+=======================+==========================================================================================================================================================================================================================================================================================================================+
   | type                  | Yes                   | Output channel type. **es** indicates that data is exported to CSS.                                                                                                                                                                                                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | region                | Yes                   | Region where CSS is located.                                                                                                                                                                                                                                                                                             |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cluster_address       | Yes                   | Private access address of the CSS cluster, for example: **x.x.x.x:x**. Use commas (,) to separate multiple addresses.                                                                                                                                                                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | es_index              | Yes                   | Index of the data to be inserted. This parameter corresponds to CSS index.                                                                                                                                                                                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | es_type               | Yes                   | Type of the document to which data is to be inserted. This parameter corresponds to the CSS type.                                                                                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                                                                                                                                          |
   |                       |                       | If the Elasticsearch version is 6.x, the value cannot start with an underscore (_).                                                                                                                                                                                                                                      |
   |                       |                       |                                                                                                                                                                                                                                                                                                                          |
   |                       |                       | If the Elasticsearch version is 7.x and the type of CSS is preset, the value must be **\_doc**. Otherwise, the value must comply with CSS specifications.                                                                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | es_fields             | Yes                   | Key of the data field to be inserted. The format is **id,f1,f2,f3,f4**. Ensure that the key corresponds to the data column in the sink. If a random attribute field instead of a key is used, the keyword **id** does not need to be used, for example, **f1,f2,f3,f4,f5**. This parameter corresponds to the CSS filed. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | batch_insert_data_num | Yes                   | Amount of data to be written in batches at a time. The value must be a positive integer. The unit is 10 records. The maximum value allowed is **65536**, and the default value is **10**.                                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | action                | No                    | If the value is **add**, data is forcibly overwritten when the same ID is encountered. If the value is **upsert**, data is updated when the same ID is encountered. (If **upsert** is selected, **id** in the **es_fields** field must be specified.) The default value is **add**.                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enable_output_null    | No                    | This parameter is used to configure whether to generate an empty field. If this parameter is set to **true**, an empty field (the value is **null**) is generated. If set to **false**, no empty field is generated. The default value is **false**.                                                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_record_num_cache  | No                    | Maximum number of records that can be cached.                                                                                                                                                                                                                                                                            |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | es_certificate_name   | No                    | Name of the datasource authentication information                                                                                                                                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                                                                                                                                          |
   |                       |                       | If the security mode is enabled and HTTPS is used by the Elasticsearch cluster, the certificate is required for access. In this case, set the datasource authentication type to **CSS**.                                                                                                                                 |
   |                       |                       |                                                                                                                                                                                                                                                                                                                          |
   |                       |                       | If the security mode is enabled for the Elasticsearch cluster but HTTPS is disabled, the certificate and username and password are required for access. In this case, set the datasource authentication type to **Password**.                                                                                            |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

If a configuration item can be specified through parameter configurations, one or more columns in the record can be used as part of the configuration item. For example, if the configuration item is set to **car_$ {car_brand}** and the value of **car_brand** in a record is **BMW**, the value of this configuration item is **car_BMW** in the record.

Example
-------

Data of stream **qualified_cars** is exported to the cluster on CSS.

::

   CREATE SINK STREAM qualified_cars (
     car_id STRING,
     car_owner STRING,
     car_age INT,
     average_speed INT,
     total_miles INT
   )
     WITH (
       type = "es",
       region = "xxx",
       cluster_address = "192.168.0.212:9200",
       es_index = "car",
       es_type = "information",
       es_fields = "id,owner,age,speed,miles",
       batch_insert_data_num = "10"
   );
