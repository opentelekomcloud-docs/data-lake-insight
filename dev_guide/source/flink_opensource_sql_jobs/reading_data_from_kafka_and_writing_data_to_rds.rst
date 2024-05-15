:original_name: dli_09_0009.html

.. _dli_09_0009:

Reading Data from Kafka and Writing Data to RDS
===============================================

.. important::

   This guide provides reference for Flink 1.12 only.

Description
-----------

In this example, we aim to query information about top three most-clicked offerings in each hour from a set of real-time click data. Offerings' real-time click data will be sent to Kafka as the input source, and then the analysis result of Kafka data is to be output to RDS.

For example, enter the following sample data:

.. code-block::

   {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 08:01:00", "product_id":"0002", "product_name":"name1"}
   {"user_id":"0002", "user_name":"Bob", "event_time":"2021-03-24 08:02:00", "product_id":"0002", "product_name":"name1"}
   {"user_id":"0002", "user_name":"Bob", "event_time":"2021-03-24 08:06:00", "product_id":"0004", "product_name":"name2"}
   {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 08:10:00", "product_id":"0003", "product_name":"name3"}
   {"user_id":"0003", "user_name":"Cindy", "event_time":"2021-03-24 08:15:00", "product_id":"0005", "product_name":"name4"}
   {"user_id":"0003", "user_name":"Cindy", "event_time":"2021-03-24 08:16:00", "product_id":"0005", "product_name":"name4"}
   {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 08:56:00", "product_id":"0004", "product_name":"name2"}
   {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 09:05:00", "product_id":"0005", "product_name":"name4"}
   {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 09:10:00", "product_id":"0006", "product_name":"name5"}
   {"user_id":"0002", "user_name":"Bob", "event_time":"2021-03-24 09:13:00", "product_id":"0006", "product_name":"name5"}

Expected output:

.. code-block::

   2021-03-24 08:00:00 - 2021-03-24 08:59:59,0002,name1,2
   2021-03-24 08:00:00 - 2021-03-24 08:59:59,0004,name2,2
   2021-03-24 08:00:00 - 2021-03-24 08:59:59,0005,name4,2
   2021-03-24 09:00:00 - 2021-03-24 09:59:59,0006,name5,2
   2021-03-24 09:00:00 - 2021-03-24 09:59:59,0005,name4,1

Prerequisites
-------------

#. You have created a DMS for Kafka instance.

   .. caution::

      When you create the instance, do not enable **Kafka SASL_SSL**.

#. You have created an RDS for MySQL DB instance.

   In this example, the RDS for MySQL database version is 8.0.

Overall Development Process
---------------------------

Overall Process


.. figure:: /_static/images/en-us_image_0000001318102237.png
   :alt: **Figure 1** Job development process

   **Figure 1** Job development process

:ref:`Step 1: Create a Queue <dli_09_0009__en-us_topic_0000001318262117_section792923214216>`

:ref:`Step 2: Create a Kafka Topic <dli_09_0009__en-us_topic_0000001318262117_section78516116518>`

:ref:`Step 3: Create an RDS Database and Table <dli_09_0009__en-us_topic_0000001318262117_section1627154113018>`

:ref:`Step 4: Create an Enhanced Datasource Connection <dli_09_0009__en-us_topic_0000001318262117_section074025752119>`

:ref:`Step 5: Run a Job <dli_09_0009__en-us_topic_0000001318262117_section12448959174212>`

:ref:`Step 6: Send Data and Query Results <dli_09_0009__en-us_topic_0000001318262117_section4387527162418>`

.. _dli_09_0009__en-us_topic_0000001318262117_section792923214216:

Step 1: Create a Queue
----------------------

#. Log in to the DLI console. In the navigation pane on the left, choose **Resources** > **Queue Management**.
#. On the displayed page, click **Buy Queue** in the upper right corner.
#. On the **Buy Queue** page, set queue parameters as follows:

   -  **Billing Mode**: .
   -  **Region** and **Project**: Retain the default values.
   -  **Name**: Enter a queue name.

      .. note::

         The queue name can contain only digits, letters, and underscores (_), but cannot contain only digits or start with an underscore (_). The name must contain 1 to 128 characters.

         **The queue name is case-insensitive. Uppercase letters will be automatically converted to lowercase letters.**

   -  **Type**: Select **For general purpose**. Select the **Dedicated Resource Mode**.
   -  **AZ Mode** and **Specifications**: Retain the default values.
   -  **Enterprise Project**: Select **default**.
   -  **Advanced Settings**: Select **Custom**.
   -  **CIDR Block**: Specify the queue network segment. For example, **10.0.0.0/16**.

      .. caution::

         The CIDR block of a queue cannot overlap with the CIDR blocks of DMS Kafka and RDS for MySQL DB instances. Otherwise, datasource connections will fail to be created.

   -  Set other parameters as required.

#. Click **Buy**. Confirm the configuration and click **Submit**.

.. _dli_09_0009__en-us_topic_0000001318262117_section78516116518:

Step 2: Create a Kafka Topic
----------------------------

#. On the Kafka management console, click an instance name on the **DMS for Kafka** page. Basic information of the Kafka instance is displayed.

#. Choose **Topics**. On the displayed page, click **Create Topic**. Configure the following parameters:

   -  Topic Name For this example, enter **testkafkatopic**.
   -  **Partitions**: Set the value to **1**.
   -  **Replicas**: Set the value to **1**.

   Retain default values for other parameters.

.. _dli_09_0009__en-us_topic_0000001318262117_section1627154113018:

Step 3: Create an RDS Database and Table
----------------------------------------

#. Log in to the RDS console. On the displayed page, locate the target MySQL DB instance and choose **More** > **Log In** in the **Operation** column.

#. On the displayed login dialog box, enter the username and password and click **Log In**.

#. On the **Databases** page, click **Create Database**. In the displayed dialog box, enter **testrdsdb** as the database name and retain default values of rest parameters. Then, click **OK**.

#. In the **Operation** column of row where the created database locates, click **SQL Window** and enter the following statement to create a table:

   .. code-block::

      CREATE TABLE clicktop (
          `range_time` VARCHAR(64) NOT NULL,
          `product_id` VARCHAR(32) NOT NULL,
          `product_name` VARCHAR(32),
          `event_count` VARCHAR(32),
          PRIMARY KEY (`range_time`,`product_id`)
      )   ENGINE = InnoDB
          DEFAULT CHARACTER SET = utf8mb4;

.. _dli_09_0009__en-us_topic_0000001318262117_section074025752119:

Step 4: Create an Enhanced Datasource Connection
------------------------------------------------

-  **Connecting DLI to Kafka**

   #. On the Kafka management console, click an instance name on the **DMS for Kafka** page. Basic information of the Kafka instance is displayed.

   #. In the **Connection** pane, obtain the **Instance Address (Private Network)**. In the **Network** pane, obtain the VPC and subnet of the instance.

   #. Click the security group name in the **Network** pane. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from DLI queues. For example, if the CIDR block of the queue is 10.0.0.0/16, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

   #. Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   #. In the displayed dialog box, set the following parameters:

      -  **Connection Name**: Enter a name for the enhanced datasource connection. For this example, enter **dli_kafka**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 1: Create a Queue <dli_09_0009__en-us_topic_0000001318262117_section792923214216>`. (Queues that are not added to a resource pool are displayed in this list.)
      -  **VPC**: Select the VPC of the Kafka instance.
      -  **Subnet**: Select the subnet of Kafka instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   #. Choose **Resources** > **Queue Management** from the navigation pane, locate the queue you created in :ref:`Step 1: Create a Queue <dli_09_0009__en-us_topic_0000001318262117_section792923214216>`. In the **Operation** column, click **More** > **Test Address Connectivity**.

   #. In the displayed dialog box, enter *Kafka instance address (private network)*\ **:**\ *port* in the **Address** box and click **Test** to check whether the instance is reachable.

-  **Connecting DLI to RDS**

   #. Go to the RDS console, click the name of the target RDS DB instance on the **Instances** page. Basic information of the instance is displayed.

   #. .. _dli_09_0009__en-us_topic_0000001318262117_li19666016361:

      In the **Connection Information** pane, obtain the floating IP address, database port, VPC, and subnet.

   #. Click the security group name. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from DLI queues. For example, if the CIDR block of the queue is 10.0.0.0/16, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

   #. Check whether the Kafka instance and RDS DB instance are in the same VPC and subnet.

      a. If they are, go to :ref:`7 <dli_09_0009__en-us_topic_0000001318262117_li9816175412318>`. You do not need to create an enhanced datasource connection again.
      b. If they are not, go to :ref:`5 <dli_09_0009__en-us_topic_0000001318262117_li11976319011>`. Create an enhanced datasource connection to connect DLI to the subnet where the RDS DB instance locates.

   #. .. _dli_09_0009__en-us_topic_0000001318262117_li11976319011:

      Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   #. In the displayed dialog box, set the following parameters:

      -  **Connection Name**: Enter a name of the enhanced datasource connection For this example, enter **dli_rds**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 1: Create a Queue <dli_09_0009__en-us_topic_0000001318262117_section792923214216>`. (Queues that are not added to a resource pool are displayed in this list.)
      -  **VPC**: Select the VPC of the RDS DB instance.
      -  **Subnet**: Select the subnet of RDS DB instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   #. .. _dli_09_0009__en-us_topic_0000001318262117_li9816175412318:

      Choose **Resources** > **Queue Management** from the navigation pane, locate the queue you created in :ref:`Step 1: Create a Queue <dli_09_0009__en-us_topic_0000001318262117_section792923214216>`. In the **Operation** column, click **More** > **Test Address Connectivity**.

   #. In the displayed dialog box, enter *floating IP address*\ **:**\ *database port* of the RDS DB instance you have obtained in :ref:`2 <dli_09_0009__en-us_topic_0000001318262117_li19666016361>` in the **Address** box and click **Test** to check whether the database is reachable.

.. _dli_09_0009__en-us_topic_0000001318262117_section12448959174212:

Step 5: Run a Job
-----------------

#. On the DLI management console, choose **Job Management** > **Flink Jobs**. On the **Flink Jobs** page, click **Create Job**.
#. In the **Create Job** dialog box, set **Type** to **Flink OpenSource SQL** and **Name** to **FlinkKafkaRds**. Click **OK**.
#. On the job editing page, set the following parameters and retain the default values of other parameters.

   -  **Queue**: Select the queue created in :ref:`Step 1: Create a Queue <dli_09_0009__en-us_topic_0000001318262117_section792923214216>`.

   -  **Flink Version**: Select **1.12**.

   -  **Save Job Log**: Enable this function.

   -  **OBS Bucket**: Select an OBS bucket for storing job logs and grant access permissions of the OBS bucket as prompted.

   -  **Enable Checkpointing**: Enable this function.

   -  Enter a SQL statement in the editing pane. The following is an example. Modify the parameters in bold as you need.

      .. note::

         In this example, the syntax version of Flink OpenSource SQL is 1.12. In this example, the data source is Kafka and the result data is written to RDS.

      .. code-block::

         create table click_product(
             user_id string, --ID of the user
             user_name string, --Username
             event_time string, --Click time
             product_id string, --Offering ID
             product_name string --Offering name
         ) with (
             "connector" = "kafka",
             "properties.bootstrap.servers" = " 10.128.0.120:9092,10.128.0.89:9092,10.128.0.83:9092 ",-- Internal network address and port number of the Kafka instance
             "properties.group.id" = "click",
             "topic" = " testkafkatopic ",--Name of the created Kafka topic
             "format" = "json",
             "scan.startup.mode" = "latest-offset"
         );

         --Result table
         create table top_product (
             range_time string, --Calculated time range
             product_id string, --Offering ID
             product_name string --Offering name
             event_count bigint, --Number of clicks
             primary key (range_time, product_id) not enforced
         ) with (
             "connector" = "jdbc",
             "url" = "jdbc:mysql://192.168.12.148:3306/testrdsdb ",--testrdsdb indicates the name of the created RDS database. Replace the IP address and port number with those of the RDS DB instance.
             "table-name" = "clicktop",
             "pwd_auth_name"="xxxxx", -- Name of the datasource authentication of the password type created on DLI. If datasource authentication is used, you do not need to set the username and password for the job.
             "sink.buffer-flush.max-rows" = "1000",
             "sink.buffer-flush.interval" = "1s"
         );

         create view current_event_view
         as
             select product_id, product_name, count(1) as click_count, concat(substring(event_time, 1, 13), ":00:00") as min_event_time, concat(substring(event_time, 1, 13), ":59:59") as max_event_time
             from click_product group by substring (event_time, 1, 13), product_id, product_name;

         insert into top_product
             select
                 concat(min_event_time, " - ", max_event_time) as range_time,
                 product_id,
                 product_name,
                 click_count
             from (
                 select *,
                 row_number() over (partition by min_event_time order by click_count desc) as row_num
                 from current_event_view
             )
             where row_num <= 3

#. Click **Check Semantic** and ensure that the SQL statement passes the check. Click **Save**. Click **Start**, confirm the job parameters, and click **Start Now** to execute the job. Wait until the job status changes to **Running**.

.. _dli_09_0009__en-us_topic_0000001318262117_section4387527162418:

Step 6: Send Data and Query Results
-----------------------------------

#. Use the Kafka client to send data to topics created in :ref:`Step 2: Create a Kafka Topic <dli_09_0009__en-us_topic_0000001318262117_section78516116518>` to simulate real-time data streams.

   The sample data is as follows:

   .. code-block::

      {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 08:01:00", "product_id":"0002", "product_name":"name1"}
      {"user_id":"0002", "user_name":"Bob", "event_time":"2021-03-24 08:02:00", "product_id":"0002", "product_name":"name1"}
      {"user_id":"0002", "user_name":"Bob", "event_time":"2021-03-24 08:06:00", "product_id":"0004", "product_name":"name2"}
      {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 08:10:00", "product_id":"0003", "product_name":"name3"}
      {"user_id":"0003", "user_name":"Cindy", "event_time":"2021-03-24 08:15:00", "product_id":"0005", "product_name":"name4"}
      {"user_id":"0003", "user_name":"Cindy", "event_time":"2021-03-24 08:16:00", "product_id":"0005", "product_name":"name4"}
      {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 08:56:00", "product_id":"0004", "product_name":"name2"}
      {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 09:05:00", "product_id":"0005", "product_name":"name4"}
      {"user_id":"0001", "user_name":"Alice", "event_time":"2021-03-24 09:10:00", "product_id":"0006", "product_name":"name5"}
      {"user_id":"0002", "user_name":"Bob", "event_time":"2021-03-24 09:13:00", "product_id":"0006", "product_name":"name5"}

#. Log in to the RDS console, click the name of the RDS DB instance. On the displayed page, click the name of the created database, for example, **testrdsdb**, and click **Query SQL Statements** in the **Operation** column of the row that containing the **clicktop** table.

   .. code-block::

      select * from `clicktop`;

#. On the displayed page, click **Execute SQL**. Check whether data has been written into the RDS table.
