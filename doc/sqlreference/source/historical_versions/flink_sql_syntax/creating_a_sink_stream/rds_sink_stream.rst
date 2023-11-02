:original_name: dli_08_0245.html

.. _dli_08_0245:

RDS Sink Stream
===============

Function
--------

DLI outputs the Flink job output data to RDS. Currently, PostgreSQL and MySQL databases are supported. The PostgreSQL database can store data of more complex types and delivers space information services, multi-version concurrent control (MVCC), and high concurrency. It applies to location applications, financial insurance, and e-commerce. The MySQL database reduces IT deployment and maintenance costs in various scenarios, such as web applications, e-commerce, enterprise applications, and mobile applications.

RDS is a cloud-based web service.

For more information about RDS, see the *Relational Database Service User Guide*.

Prerequisites
-------------

-  Ensure that you have created a PostgreSQL or MySQL RDS instance in RDS.

   For details about how to create an RDS instance, see **Creating an Instance** in the *Relational Database Service User Guide*.

-  In this scenario, jobs must run on the dedicated queue of DLI. Therefore, DLI must interconnect with the enhanced datasource connection that has been connected with RDS instance. You can also set the security group rules as required.

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
   | type                  | Yes                   | Output channel type. **rds** indicates that data is exported to RDS.                                                                                                                                                                                                                                      |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username              | Yes                   | Username for connecting to a database.                                                                                                                                                                                                                                                                    |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password              | Yes                   | Password for connecting to a database.                                                                                                                                                                                                                                                                    |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_url                | Yes                   | Database connection address, for example, **{database_type}://ip:port/database**.                                                                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | Currently, two types of database connections are supported: MySQL and PostgreSQL.                                                                                                                                                                                                                         |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | -  MySQL: 'mysql://ip:port/database'                                                                                                                                                                                                                                                                      |
   |                       |                       | -  PostgreSQL: 'postgresql://ip:port/database'                                                                                                                                                                                                                                                            |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes                   | Name of the table where data will be inserted.                                                                                                                                                                                                                                                            |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_columns            | No                    | Mapping between attributes in the output stream and those in the database table. This parameter must be configured based on the sequence of attributes in the output stream.                                                                                                                              |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | Example:                                                                                                                                                                                                                                                                                                  |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       | .. code-block::                                                                                                                                                                                                                                                                                           |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       |    create sink stream a3(student_name  string, student_age int) with (                                                                                                                                                                                                                                    |
   |                       |                       |    type = "rds",                                                                                                                                                                                                                                                                                          |
   |                       |                       |    username = "root",                                                                                                                                                                                                                                                                                     |
   |                       |                       |    password = "xxxxxxxx",                                                                                                                                                                                                                                                                                 |
   |                       |                       |    db_url = "mysql://192.168.0.102:8635/test1",                                                                                                                                                                                                                                                           |
   |                       |                       |    db_columns = "name,age",                                                                                                                                                                                                                                                                               |
   |                       |                       |    table_name = "t1"                                                                                                                                                                                                                                                                                      |
   |                       |                       |    );                                                                                                                                                                                                                                                                                                     |
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
   |                       |                       | .. code-block::                                                                                                                                                                                                                                                                                           |
   |                       |                       |                                                                                                                                                                                                                                                                                                           |
   |                       |                       |    CREATE SINK STREAM test(c_timeminute LONG, c_cnt LONG)                                                                                                                                                                                                                                                 |
   |                       |                       |    WITH (                                                                                                                                                                                                                                                                                                 |
   |                       |                       |    type = "rds",                                                                                                                                                                                                                                                                                          |
   |                       |                       |    username = "root",                                                                                                                                                                                                                                                                                     |
   |                       |                       |    password = "xxxxxxxx",                                                                                                                                                                                                                                                                                 |
   |                       |                       |    db_url = "mysql://192.168.0.12:8635/test",                                                                                                                                                                                                                                                             |
   |                       |                       |    table_name = "test",                                                                                                                                                                                                                                                                                   |
   |                       |                       |    primary_key = "c_timeminute");                                                                                                                                                                                                                                                                         |
   +-----------------------+-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | operation_field       | No                    | Processing method of specified data in the format of ${field_name}. The value of **field_name** must be a string. If **field_name** indicates D or DELETE, this record is deleted from the database and data is inserted by default.                                                                      |
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
       db_url = "mysql://192.168.1.1:8635/test",
       table_name = "audi_cheaper_than_30w"
   );
