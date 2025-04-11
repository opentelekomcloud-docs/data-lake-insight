:original_name: dli_08_15104.html

.. _dli_08_15104:

GaussDB(DWS) Source Table (Not Recommended)
===========================================

Function
--------

DLI reads data of Flink jobs from GaussDB(DWS). GaussDB(DWS) database kernel is compliant with PostgreSQL. The PostgreSQL database can store data of more complex types and deliver space information services, multi-version concurrent control (MVCC), and high concurrency. It applies to location applications, financial insurance, and e-Commerce.

GaussDB(DWS) is an online data processing database based on the cloud infrastructure and platform and helps you mine and analyze massive sets of data.

.. note::

   You are advised to use GaussDB(DWS) self-developed GaussDB(DWS) connector.

Prerequisites
-------------

-  You have created a GaussDB(DWS) cluster.

   For details about how to create a GaussDB(DWS) cluster, see **Creating a Cluster** in the *Data Warehouse Service Management Guide*.

-  You have created a GaussDB(DWS) database table.

-  An enhanced datasource connection has been created for DLI to connect to GaussDB(DWS) clusters, so that jobs can run on the dedicated queue of DLI and you can set the security group rules as required.

Precautions
-----------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  Storing authentication credentials such as usernames and passwords in code or plaintext poses significant security risks. It is recommended using DEW to manage credentials instead. Storing encrypted credentials in configuration files or environment variables and decrypting them when needed ensures security. For details, see .
-  Fields in the **with** parameter can only be enclosed in single quotes.

Syntax
------

::

   create table dwsSource (
     attr_name attr_type
     (',' attr_name attr_type)*
     (','PRIMARY KEY (attr_name, ...) NOT ENFORCED)
     (',' watermark for rowtime_column_name as watermark-strategy_expression)
   )
   with (
     'connector' = 'gaussdb',
     'url' = '',
     'table-name' = '',
     'username' = '',
     'password' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Mandatory   | Default Value         | Data Type   | Description                                                                                                                                                                                                             |
   +============================+=============+=======================+=============+=========================================================================================================================================================================================================================+
   | connector                  | Yes         | None                  | String      | Connector to be used. Set this parameter to **gaussdb**.                                                                                                                                                                |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | url                        | Yes         | None                  | String      | JDBC connection address. Set the IP address in this parameter to the internal IP address of GaussDB(DWS).                                                                                                               |
   |                            |             |                       |             |                                                                                                                                                                                                                         |
   |                            |             |                       |             | If you use the gsjdbc4 driver, set the value in jdbc:postgresql://${ip}:${port}/${dbName} format.                                                                                                                       |
   |                            |             |                       |             |                                                                                                                                                                                                                         |
   |                            |             |                       |             | If you use the gsjdbc200 driver, set the value in jdbc:gaussdb://${ip}:${port}/${dbName} format.                                                                                                                        |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table-name                 | Yes         | None                  | String      | Name of the GaussDB(DWS) table to be operated. If the GaussDB(DWS) table is in a schema, refer to the description of :ref:`GaussDB(DWS) table in a schema <dli_08_15104__en-us_topic_0000001262495782_li032114411215>`. |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | driver                     | No          | org.postgresql.Driver | String      | JDBC connection driver. The default value is **org.postgresql.Driver**.                                                                                                                                                 |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | username                   | No          | None                  | String      | Username for GaussDB(DWS) database authentication. This parameter must be configured in pair with **password**.                                                                                                         |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | password                   | No          | None                  | String      | Password for GaussDB(DWS) database authentication. This parameter must be configured in pair with **username**.                                                                                                         |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.column      | No          | None                  | String      | Name of the column used to partition the input.                                                                                                                                                                         |
   |                            |             |                       |             |                                                                                                                                                                                                                         |
   |                            |             |                       |             | Note: This parameter must be used together with **scan.partition.lower-bound**, **scan.partition.upper-bound**, and **scan.partition.num**.                                                                             |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.lower-bound | No          | None                  | Integer     | Lower bound of values to be fetched for the first partition.                                                                                                                                                            |
   |                            |             |                       |             |                                                                                                                                                                                                                         |
   |                            |             |                       |             | This parameter must be used together with **scan.partition.column**, **scan.partition.upper-bound**, and **scan.partition.num**.                                                                                        |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.upper-bound | No          | None                  | Integer     | Upper bound of values to be fetched for the last partition.                                                                                                                                                             |
   |                            |             |                       |             |                                                                                                                                                                                                                         |
   |                            |             |                       |             | This parameter must be used together with **scan.partition.column**, **scan.partition.lower-bound**, and **scan.partition.num**.                                                                                        |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.partition.num         | No          | None                  | Integer     | Number of partitions to be created.                                                                                                                                                                                     |
   |                            |             |                       |             |                                                                                                                                                                                                                         |
   |                            |             |                       |             | This parameter must be used together with **scan.partition.column**, **scan.partition.upper-bound**, and **scan.partition.upper-bound**.                                                                                |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | scan.fetch-size            | No          | 0                     | Integer     | Number of rows fetched from the database each time. The default value is **0**, indicating that the number of rows is not limited.                                                                                      |
   +----------------------------+-------------+-----------------------+-------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

In this example, data is read from the GaussDB(DWS) data source and written to the Print result table. The procedure is as follows:

#. Create a table named **dws_order** in GaussDB(DWS).

   .. code-block::

      create table public.dws_order(
        order_id VARCHAR,
        order_channel VARCHAR,
        order_time VARCHAR,
        pay_amount FLOAT8,
        real_pay FLOAT8,
        pay_time VARCHAR,
        user_id VARCHAR,
        user_name VARCHAR,
        area_id VARCHAR);

   Insert data into the **dws_order** table.

   .. code-block::

      insert into public.dws_order
        (order_id,
        order_channel,
        order_time,
        pay_amount,
        real_pay,
        pay_time,
        user_id,
        user_name,
        area_id) values
        ('202103241000000001', 'webShop', '2021-03-24 10:00:00', '100.00', '100.00', '2021-03-24 10:02:03', '0001', 'Alice', '330106'),
        ('202103251202020001', 'miniAppShop', '2021-03-25 12:02:02', '60.00', '60.00', '2021-03-25 12:03:00', '0002', 'Bob', '330110');

#. Create an enhanced datasource connection in the VPC and subnet where GaussDB(DWS) locates, and bind the connection to the required Flink elastic resource pool.

#. Set GaussDB(DWS) security groups and add inbound rules to allow access from the Flink queue. Test the connectivity using the GaussDB(DWS) address. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Create a Flink OpenSource SQL job. Enter the following job script and submit the job. The job script uses the GaussDB(DWS) data source and the Print result table.

   When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      CREATE TABLE dwsSource (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'gaussdb',
        'url' = 'jdbc:postgresql://DWSIP:DWSPort/DWSdbName',
        'table-name' = 'dws_order',
        'driver' = 'org.postgresql.Driver',
        'username' = 'DWSUserName',
        'password' = 'DWSPassword'
      );

      CREATE TABLE printSink (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'print'
      );

      insert into printSink select * from dwsSource;

#. Perform the following operations to view the data result in the **taskmanager.out** file:

   a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The data result is as follows:

   .. code-block::

      +I(202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106)
      +I(202103251202020001,miniAppShop,2021-03-25 12:02:02,60.0,60.0,2021-03-25 12:03:00,0002,Bob,330110)

FAQ
---

-  Q: What should I do if the job execution fails and the log contains the following error information?

   .. code-block::

      java.io.IOException: unable to open JDBC writer
      ...
      Caused by: org.postgresql.util.PSQLException: The connection attempt failed.
      ...
      Caused by: java.net.SocketTimeoutException: connect timed out

   A: The datasource connection is not bound or the binding fails.

-  .. _dli_08_15104__en-us_topic_0000001262495782_li032114411215:

   Q: How can I configure a GaussDB(DWS) table that is in a schema?

   A: The following provides an example of configuring the **dws_order** table in the **dbuser2** schema:

   .. code-block::

      CREATE TABLE dwsSource (
        order_id string,
        order_channel string,
        order_time string,
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string
      ) WITH (
        'connector' = 'gaussdb',
        'url' = 'jdbc:postgresql://DWSIP:DWSPort/DWSdbName',
        'table-name' = 'dbuser2.dws_order',
        'driver' = 'org.postgresql.Driver',
        'username' = 'DWSUserName',
        'password' = 'DWSPassword'
      );
