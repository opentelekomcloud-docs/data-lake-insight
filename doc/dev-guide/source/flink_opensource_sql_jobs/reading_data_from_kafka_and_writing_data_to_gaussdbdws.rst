:original_name: dli_09_0010.html

.. _dli_09_0010:

Reading Data from Kafka and Writing Data to GaussDB(DWS)
========================================================

.. important::

   This guide provides reference for Flink 1.12 only.

Description
-----------

This example analyzes real-time vehicle driving data and collects statistics on data results that meet specific conditions. The real-time vehicle driving data is stored in the Kafka source table, and then the analysis result is output to GaussDB(DWS).

For example, enter the following sample data:

.. code-block::

   {"car_id":"3027", "car_owner":"lilei", "car_age":"7", "average_speed":"76", "total_miles":"15000"}
   {"car_id":"3028", "car_owner":"hanmeimei", "car_age":"6", "average_speed":"92", "total_miles":"17000"}
   {"car_id":"3029", "car_owner":"Ann", "car_age":"10", "average_speed":"81", "total_miles":"230000"}

Expected output is vehicles meeting the average_speed <= 90 and total_miles <= 200,000 condition.

.. code-block::

   {"car_id":"3027", "car_owner":"lilei", "car_age":"7", "average_speed":"76", "total_miles":"15000"}

Prerequisites
-------------

#. You have created a DMS for Kafka instance.

   .. caution::

      When you create the instance, do not enable **Kafka SASL_SSL**.

#. You have created a GaussDB(DWS) instance.

Overall Development Process
---------------------------

Overall Process


.. figure:: /_static/images/en-us_image_0000001318262121.png
   :alt: **Figure 1** Job development process

   **Figure 1** Job development process

:ref:`Step 1: Create a Queue <dli_09_0010__en-us_topic_0000001269182164_section792923214216>`

:ref:`Step 2: Create a Kafka Topic <dli_09_0010__en-us_topic_0000001269182164_section78516116518>`

:ref:`Step 3: Create a GaussDB(DWS) Database and Table <dli_09_0010__en-us_topic_0000001269182164_section1627154113018>`

:ref:`Step 4: Create an Enhanced Datasource Connection <dli_09_0010__en-us_topic_0000001269182164_section074025752119>`

:ref:`Step 5: Run a Job <dli_09_0010__en-us_topic_0000001269182164_section12448959174212>`

:ref:`Step 6: Send Data and Query Results <dli_09_0010__en-us_topic_0000001269182164_section4387527162418>`

.. _dli_09_0010__en-us_topic_0000001269182164_section792923214216:

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

.. _dli_09_0010__en-us_topic_0000001269182164_section78516116518:

Step 2: Create a Kafka Topic
----------------------------

#. On the Kafka management console, click an instance name on the **DMS for Kafka** page. Basic information of the Kafka instance is displayed.

#. Choose **Topics** in the navigation pane on the left. On the displayed page, click **Create Topic**. Configure the following parameters:

   -  **Topic Name**: For this example, enter **testkafkatopic**.
   -  **Partitions**: Set the value to **1**.
   -  **Replicas**: Set the value to **1**.

   Retain default values for other parameters.

.. _dli_09_0010__en-us_topic_0000001269182164_section1627154113018:

Step 3: Create a GaussDB(DWS) Database and Table
------------------------------------------------

#. .

#. Connect to the default database **gaussdb** of a GaussDB(DWS) cluster.

   .. code-block::

      gsql -d gaussdb -h Connection address of the GaussDB(DWS) cluster -U dbadmin -p 8000 -W password -r

   -  **gaussdb**: Default database of the GaussDB(DWS) cluster
   -  **Connection address of the DWS cluster**: If a public network address is used for connection, set this parameter to the public network IP address or domain name. If a private network address is used for connection, set this parameter to the private network IP address or domain name. If an ELB is used for connection, set this parameter to the ELB address.
   -  **dbadmin**: Default administrator username used during cluster creation
   -  **password**: Default password of the administrator

#. Run the following command to create the **testdwsdb** database:

   .. code-block::

      CREATE DATABASE testdwsdb;

#. Run the following command to exit the **gaussdb** database and connect to **testdwsdb**:

   .. code-block::

      \q
      gsql -d testdwsdb -h Connection address of the GaussDB(DWS) cluster -U dbadmin -p 8000 -W password -r

#. Run the following commands to create a table:

   .. code-block::

      create schema test;
      set current_schema= test;
      drop table if exists qualified_cars;
      CREATE TABLE qualified_cars
      (
          car_id VARCHAR,
          car_owner VARCHAR,
          car_age INTEGER ,
          average_speed FLOAT8,
          total_miles FLOAT8
      );

.. _dli_09_0010__en-us_topic_0000001269182164_section074025752119:

Step 4: Create an Enhanced Datasource Connection
------------------------------------------------

-  **Connecting DLI to Kafka**

   #. On the Kafka management console, click an instance name on the **DMS for Kafka** page. Basic information of the Kafka instance is displayed.

   #. In the **Connection** pane, obtain the **Instance Address (Private Network)**. In the **Network** pane, obtain the VPC and subnet of the instance.

   #. Click the security group name in the **Network** pane. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from DLI queues. For example, if the CIDR block of the queue is 10.0.0.0/16, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

   #. Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   #. In the displayed dialog box, set the following parameters: For details, see the following section:

      -  **Connection Name**: Enter a name for the enhanced datasource connection. For this example, enter **dli_kafka**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 1: Create a Queue <dli_09_0010__en-us_topic_0000001269182164_section792923214216>`.
      -  **VPC**: Select the VPC of the Kafka instance.
      -  **Subnet**: Select the subnet of Kafka instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   #. Choose **Resources** > **Queue Management** from the navigation pane, locate the queue you created in :ref:`Step 1: Create a Queue <dli_09_0010__en-us_topic_0000001269182164_section792923214216>`. In the **Operation** column, click **More** > **Test Address Connectivity**.

   #. In the displayed dialog box, enter *Kafka instance address (private network)*\ **:**\ *port* in the **Address** box and click **Test** to check whether the instance is reachable.

-  **Connecting DLI to GaussDB(DWS)**

   #. On the GaussDB(DWS) management console, choose **Clusters**. On the displayed page, click the name of the created GaussDB(DWS) cluster to view basic information.

   #. .. _dli_09_0010__en-us_topic_0000001269182164_li19666016361:

      In the Basic Information tab, locate the **Database Attributes** pane and obtain the private IP address and port number of the DB instance. In the **Network** pane, obtain VPC, and subnet information.

   #. Click the security group name. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from DLI queues. For example, if the CIDR block of the queue is 10.0.0.0/16, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

   #. Check whether the Kafka instance and GaussDB(DWS) instance are in the same VPC and subnet.

      a. If they are, go to :ref:`7 <dli_09_0010__en-us_topic_0000001269182164_li9816175412318>`. You do not need to create an enhanced datasource connection again.
      b. If they are not, go to :ref:`5 <dli_09_0010__en-us_topic_0000001269182164_li11976319011>`. Create an enhanced datasource connection to connect DLI to the subnet where the GaussDB(DWS) instance locates.

   #. .. _dli_09_0010__en-us_topic_0000001269182164_li11976319011:

      Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   #. In the displayed dialog box, set the following parameters: For details, see the following section:

      -  **Connection Name**: Enter a name for the enhanced datasource connection. For this example, enter **dli_dws**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 1: Create a Queue <dli_09_0010__en-us_topic_0000001269182164_section792923214216>`.
      -  **VPC**: Select the VPC of the GaussDB(DWS) instance.
      -  **Subnet**: Select the subnet of GaussDB(DWS) instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   #. .. _dli_09_0010__en-us_topic_0000001269182164_li9816175412318:

      Choose **Resources** > **Queue Management** from the navigation pane, locate the queue you created in :ref:`Step 1: Create a Queue <dli_09_0010__en-us_topic_0000001269182164_section792923214216>`. In the **Operation** column, click **More** > **Test Address Connectivity**.

   #. In the displayed dialog box, enter *floating IP address*\ **:**\ *database port* of the GaussDB(DWS) instance you have obtained in :ref:`2 <dli_09_0010__en-us_topic_0000001269182164_li19666016361>` in the **Address** box and click **Test** to check whether the database is reachable.

.. _dli_09_0010__en-us_topic_0000001269182164_section12448959174212:

Step 5: Run a Job
-----------------

#. On the DLI management console, choose **Job Management** > **Flink Jobs**. On the **Flink Jobs** page, click **Create Job**.
#. In the **Create Job** dialog box, set **Type** to **Flink OpenSource SQL** and **Name** to **FlinkKafkaDWS**. Click **OK**.
#. On the job editing page, set the following parameters and retain the default values of other parameters.

   -  **Queue**: Select the queue created in :ref:`Step 1: Create a Queue <dli_09_0010__en-us_topic_0000001269182164_section792923214216>`.

   -  **Flink Version**: Select **1.12**.

   -  **Save Job Log**: Enable this function.

   -  **OBS Bucket**: Select an OBS bucket for storing job logs and grant access permissions of the OBS bucket as prompted.

   -  **Enable Checkpointing**: Enable this function.

   -  Enter a SQL statement in the editing pane. The following is an example. Modify the parameters in bold as you need.

      .. note::

         In this example, the syntax version of Flink OpenSource SQL is 1.12. In this example, the data source is Kafka and the result data is written to GaussDB(DWS).

      .. code-block::

         create table car_infos(
           car_id STRING,
           car_owner STRING,
           car_age INT,
           average_speed DOUBLE,
           total_miles DOUBLE
         ) with (
             "connector" = "kafka",
             "properties.bootstrap.servers" = " 10.128.0.120:9092,10.128.0.89:9092,10.128.0.83:9092 ",-- Internal network address and port number of the Kafka instance
             "properties.group.id" = "click",
             "topic" = " testkafkatopic",--Created Kafka topic
             "format" = "json",
             "scan.startup.mode" = "latest-offset"
         );

         create table qualified_cars (
           car_id STRING,
           car_owner STRING,
           car_age INT,
           average_speed DOUBLE,
           total_miles DOUBLE
         )
         WITH (
           'connector' = 'gaussdb',
           'driver' = 'com.gauss200.jdbc.Driver',
           'url'='jdbc:gaussdb://192.168.168.16:8000/testdwsdb ', ---192.168.168.16:8000 indicates the internal IP address and port of the GaussDB(DWS) instance. testdwsdb indicates the name of the created GaussDB(DWS) database.
           'table-name' = ' test\".\"qualified_cars', ---test indicates the schema of the created GaussDB(DWS) table, and qualified_cars indicates the GaussDB(DWS) table name.
           'pwd_auth_name'= 'xxxxx', -- Name of the datasource authentication of the password type created on DLI. If datasource authentication is used, you do not need to set the username and password for the job.
           'write.mode' = 'insert'
         );

         /** Output information about qualified vehicles **/
         INSERT INTO qualified_cars
         SELECT *
         FROM car_infos
         where average_speed <= 90 and total_miles <= 200000;

#. Click **Check Semantic** and ensure that the SQL statement passes the check. Click **Save**. Click **Start**, confirm the job parameters, and click **Start Now** to execute the job. Wait until the job status changes to **Running**.

.. _dli_09_0010__en-us_topic_0000001269182164_section4387527162418:

Step 6: Send Data and Query Results
-----------------------------------

#. Use the Kafka client to send data to topics created in :ref:`Step 2: Create a Kafka Topic <dli_09_0010__en-us_topic_0000001269182164_section78516116518>` to simulate real-time data streams.

   The sample data is as follows:

   .. code-block::

      {"car_id":"3027", "car_owner":"lilei", "car_age":"7", "average_speed":"76", "total_miles":"15000"}
      {"car_id":"3028", "car_owner":"hanmeimei", "car_age":"6", "average_speed":"92", "total_miles":"17000"}
      {"car_id":"3029", "car_owner":"Ann", "car_age":"10", "average_speed":"81", "total_miles":"230000"}

#. Connect to the created GaussDB(DWS) cluster.

#. Connect to the default database **testdwsdb** of a GaussDB(DWS) cluster.

   .. code-block::

      gsql -d testdwsdb -h Connection address of the GaussDB(DWS) cluster -U dbadmin -p 8000 -W password -r

#. Run the following statement to query GaussDB(DWS) table data:

   .. code-block::

      select * from test.qualified_cars;

   The query result is as follows:

   .. code-block::

      car_id  car_owner  car_age  average_speed  total_miles
      3027      lilei     7           76.0       15000.0
