:original_name: dli_08_0247.html

.. _dli_08_0247:

DWS Sink Stream (JDBC Mode)
===========================

Function
--------

DLI outputs the Flink job output data to Data Warehouse Service (DWS). DWS database kernel is compliant with PostgreSQL. The PostgreSQL database can store data of more complex types and delivers space information services, multi-version concurrent control (MVCC), and high concurrency. It applies to location applications, financial insurance, and e-commerce.

DWS is an online data processing database based on the cloud infrastructure and platform and helps you mine and analyze massive sets of data. For more information about DWS, see the .

Prerequisites
-------------

-  Ensure that you have created a DWS cluster on DWS using your account.

   For details about how to create a DWS cluster, see **Creating a Cluster** in the *Data Warehouse Service Management Guide*.

-  Ensure that a DWS database table has been created.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with DWS clusters. You can also set the security group rules as required.

   For details about how to create an enhanced datasource connection, see **Enhanced Datasource Connections** in the *Data Lake Insight User Guide*.

   For details about how to configure security group rules, see **Security Group** in the *Virtual Private Cloud User Guide*.

Syntax
------

::

   CREATE SINK STREAM stream_id (attr_name attr_type (',' attr_name attr_type)* )
     WITH (
       type = "rds",
       username = "",
       password = "",
       db_url = "",
       table_name = ""
     );

Keyword
-------

.. table:: **Table 1** Keyword description

   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                               |
   +=======================+=======================+===========================================================================================================================================================================================================================================================================================================+
   | type                  | Yes                   | Output channel type. **rds** indicates that data is exported to RDS or DWS.                                                                                                                                                                                                                               |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username              | Yes                   | Username for connecting to a database.                                                                                                                                                                                                                                                                    |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password              | Yes                   | Password for connecting to a database.                                                                                                                                                                                                                                                                    |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_url                | Yes                   | Database connection address, for example, **postgresql://ip:port/database**.                                                                                                                                                                                                                              |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes                   | Name of the table where data will be inserted. You need to create the database table in advance.                                                                                                                                                                                                          |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_columns            | No                    | Mapping between attributes in the output stream and those in the database table. This parameter must be configured based on the sequence of attributes in the output stream.                                                                                                                              |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | Example:                                                                                                                                                                                                                                                                                                  |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | ::                                                                                                                                                                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       |    create sink stream a3(student_name  string, student_age int)                                                                                                                                                                                                                                           |
   |                       |                       |      with (                                                                                                                                                                                                                                                                                               |
   |                       |                       |        type = "rds",                                                                                                                                                                                                                                                                                      |
   |                       |                       |        username = "root",                                                                                                                                                                                                                                                                                 |
   |                       |                       |        password = "xxxxxxxx",                                                                                                                                                                                                                                                                             |
   |                       |                       |        db_url = "postgresql://192.168.0.102:8000/test1",                                                                                                                                                                                                                                                  |
   |                       |                       |        db_columns = "name,age",                                                                                                                                                                                                                                                                           |
   |                       |                       |        table_name = "t1"                                                                                                                                                                                                                                                                                  |
   |                       |                       |      );                                                                                                                                                                                                                                                                                                   |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | In the example, **student_name** corresponds to the name attribute in the database, and **student_age** corresponds to the age attribute in the database.                                                                                                                                                 |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | .. note::                                                                                                                                                                                                                                                                                                 |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       |    -  If **db_columns** is not configured, it is normal that the number of attributes in the output stream is less than that of attributes in the database table and the extra attributes in the database table are all nullable or have default values.                                                  |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | primary_key           | No                    | To update data in the table in real time by using the primary key, add the **primary_key** configuration item (**c_timeminute** in the following example) when creating a table. During the data writing operation, data is updated if the specified **primary_key** exists. Otherwise, data is inserted. |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | Example:                                                                                                                                                                                                                                                                                                  |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | ::                                                                                                                                                                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       |    CREATE SINK STREAM test(c_timeminute LONG, c_cnt LONG)                                                                                                                                                                                                                                                 |
   |                       |                       |      WITH (                                                                                                                                                                                                                                                                                               |
   |                       |                       |        type = "rds",                                                                                                                                                                                                                                                                                      |
   |                       |                       |        username = "root",                                                                                                                                                                                                                                                                                 |
   |                       |                       |        password = "xxxxxxxx",                                                                                                                                                                                                                                                                             |
   |                       |                       |        db_url = "postgresql://192.168.0.12:8000/test",                                                                                                                                                                                                                                                    |
   |                       |                       |        table_name = "test",                                                                                                                                                                                                                                                                               |
   |                       |                       |        primary_key = "c_timeminute"                                                                                                                                                                                                                                                                       |
   |                       |                       |      );                                                                                                                                                                                                                                                                                                   |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

The stream format defined by **stream_id** must be the same as the table format.

Example
-------

Data of stream **audi_cheaper_than_30w** is exported to the **audi_cheaper_than_30w** table in the **test** database.

::

   CREATE SINK STREAM audi_cheaper_than_30w (
     car_id STRING,
     car_owner STRING,
     car_brand STRING,
     car_price INT
   )
     WITH (
       type = "rds",
       username = "root",
       password = "xxxxxx",
       db_url = "postgresql://192.168.1.1:8000/test",
       table_name = "audi_cheaper_than_30w"
     );

   insert into audi_cheaper_than_30w select "1","2","3",4;
