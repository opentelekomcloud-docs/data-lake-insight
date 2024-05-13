:original_name: dli_01_0531.html

.. _dli_01_0531:

Creating and Submitting a Flink OpenSource SQL Job
==================================================

Scenario
--------

DLI Flink jobs can use other cloud services as data sources and sink streams for real-time compute. This example describes how to create and submit a Flink Opensource SQL job that uses Kafka as the input stream and RDS as the output stream.

Procedure
---------

You need to create a Flink OpenSource SQL job that has an input stream and an output stream. The input stream reads data from Kafka, and the output stream writes data into RDS. The procedure is as follows:

:ref:`Step 1: Prepare a Data Source <dli_01_0531__en-us_topic_0000001354966081_section1133722963618>`

:ref:`Step 2: Prepare a Data Output Channel <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section10891114913473>`

:ref:`Step 3: Create an OBS Bucket to Store Output Data <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section18912701608>`

:ref:`Step 4: Create a Queue <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`

:ref:`Step 5: Create an Enhanced Datasource Connection Between DLI and Kafka <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section19012773105034>`

:ref:`Step 6: Create an Enhanced Datasource Connection Between DLI and RDS <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section21433273112656>`

:ref:`Step 7: Create a Flink OpenSource SQL Job <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section21590507141153>`

.. _dli_01_0531__en-us_topic_0000001354966081_section1133722963618:

Step 1: Prepare a Data Source
-----------------------------

In this example, Kafka is the data source.

For more information about Flink job data, see :ref:`Preparing Flink Job Data <dli_01_0454>`.

Enable DIS to import Kafka data to DLI. For details, see "Buying a Kafka Instance" in the *Distributed Message Service Kafka User Guide*.

#. .. _dli_01_0531__en-us_topic_0000001354966081_li485218325375:

   Create the dependent Kafka resources.

   Before creating a Kafka instance, ensure the availability of resources, including a virtual private cloud (VPC), subnet, security group, and security group rules.

   -  For details about how to create a VPC and subnet, see "Creating a VPC and Subnet" in *Virtual Private Cloud User Guide*. For details about how to create and use a subnet in an existing VPC, see "Create a Subnet for the VPC" in *Virtual Private Cloud User Guide*.

      .. note::

         -  The created VPC and the Kafka instance you will create must be in the same region.
         -  Retain the default settings unless otherwise specified.

   -  For details about how to create a security group, see "Creating a Security Group" in the *Virtual Private Cloud User Guide*. For details about how to add rules to a security group, see "Creating a Subnet for the VPC" in the *Virtual Private Cloud User Guide*.

   For more information, see in *Distributed Message Service for Kafka User Guide*.

#. Create a DMS for Kafka Instance for job input streams.

   a. Log in to the DMS for Kafka console.
   b. Select a region in the upper left corner.
   c. On the **DMS for Kafka** page, click **Buy Instance** in the upper right corner and set related parameters. The required instance information is as follows:

      -  **Region**: Select the region where DLI is located.
      -  **Project**: Keep the default value.
      -  **AZ**: Keep the default value.
      -  **Instance Name**: **kafka-dliflink**
      -  **Specifications**: **Default**
      -  **Enterprise Project**: **default**
      -  **Version**: Keep the default value.
      -  **CPU Architecture**: Keep the default value.
      -  **Broker Flavor**: Select a flavor as needed.
      -  **Brokers**: Retain the default value.
      -  **Storage Space**: Keep the default value.
      -  **Capacity Threshold Policy**: Keep the default value.
      -  **VPC** and **Subnet**: Select the VPC and subnet created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`.
      -  **Security Group**: Select the security group created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`.
      -  **Manager Username**: Enter **dliflink** (used to log in to the instance management page).
      -  **Password**: \***\* (The system cannot detect your password.)
      -  **Confirm Password**: \***\*
      -  **More Settings**: Do not configure this parameter.

   d. Click **Buy**. The confirmation page is displayed.
   e. Confirm that the instance information is correct, read and agree to the , and click **Submit**. It takes about 10 to 15 minutes to create an instance.

#. Create a Kafka topic.

   a. Click the name of the created Kafka instance. The basic information page of the instance is displayed.

   b. Choose **Topics** in the navigation pane on the left. On the displayed page, click **Create Topic**. Configure the following parameters:

      -  **Topic Name**: For this example, enter **testkafkatopic**.
      -  **Partitions**: Set the value to **1**.
      -  **Replicas**: Set the value to **1**.

      Retain the default values for other parameters.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section10891114913473:

Step 2: Prepare a Data Output Channel
-------------------------------------

To use RDS as the data output channel, create an RDS MySQL instance. For details, see "Getting Started with RDS for MySQL" in *Getting Started with Relational Database Service*.

#. Log in to the RDS management console.

#. Select a region in the upper left corner.

#. Click **Buy DB Instance** in the upper right corner of the page and set related parameters. Retain the default values for other parameters.

   -  **Region**: Select the region where DLI is located.
   -  **DB Instance Name**: Enter **rds-dliflink**.
   -  **DB Engine**: Select **MySQL**.
   -  **DB Engine Version**: Select **8.0**.
   -  **DB Instance Type**: Select **Primary/Standby**.
   -  **Storage Type**: Cloud SSD may be selected by default.
   -  **Primary AZ**: Select a custom AZ.
   -  **Standby AZ**: Select a custom AZ.
   -  **Instance Class**: Select a class as needed and choose **2 vCPUs \| 8 GB**.
   -  **Storage Space (GB)**: Set it to **40**.
   -  **VPC**: Select the VPC and subnet created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`.
   -  **Database Port**: Enter **3306**.
   -  **Security Group**: Select the security group created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`.
   -  **Administrator Password**: \***\* (Keep the password secure. The system cannot retrieve your password.)
   -  **Confirm Password**: \***\*
   -  **Parameter Template**: Choose **Default-MySQL-8.0**.

#. Click **Next** and confirm the specifications.

#. Click **Submit**. The RDS DB instance is created.

#. Log in to the MySQL database and create table **orders** in database **flink**.

   Log in to the MySQL instance, click the **flink** database. On the displayed page, click **SQL Window**. Enter the following table creation statement in the SQL editing pane to create a table.

   .. code-block::

      CREATE TABLE `flink`.`orders` (
          `order_id` VARCHAR(32) NOT NULL,
          `order_channel` VARCHAR(32) NULL,
          `order_time` VARCHAR(32) NULL,
          `pay_amount` DOUBLE UNSIGNED NOT NULL,
          `real_pay` DOUBLE UNSIGNED NULL,
          `pay_time` VARCHAR(32) NULL,
          `user_id` VARCHAR(32) NULL,
          `user_name` VARCHAR(32) NULL,
          `area_id` VARCHAR(32) NULL,
          PRIMARY KEY (`order_id`)
      )   ENGINE = InnoDB
          DEFAULT CHARACTER SET = utf8mb4
          COLLATE = utf8mb4_general_ci;

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section18912701608:

Step 3: Create an OBS Bucket to Store Output Data
-------------------------------------------------

In this step, you need to enable OBS for **JobSample** and enable checkpointing for the DLI Flink job to save job logs and test data storage.

For details about how to create a bucket, see "Creating a Bucket" in the *Object Storage Service Console Operation Guide*.

#. In the navigation pane on the OBS management console, choose **Object Storage**.
#. In the upper right corner of the page, click **Create Bucket** and set bucket parameters.

   -  **Region**: Select the region where DLI is located.
   -  **Bucket Name**: Enter a bucket name. For this example, enter **obstest**.
   -  **Default Storage Class**: **Standard**
   -  **Bucket Policy**: **Private**
   -  **Default Encryption**: **Do not enable**
   -  **Direct Reading**: **Do not enable**
   -  **Enterprise Project**: **default**
   -  **Tags**: Leave it blank.

#. Click **Create Now**.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710:

Step 4: Create a Queue
----------------------

Flink OpenSource SQL jobs cannot run on the default queue. You need to create a queue, for example, **Flinktest**. For details, see "Creating a Queue".

#. Log in to the DLI management console. On the **Overview** page, click **Buy Queue** in the upper right corner.

   If this is your first time logging in to the DLI management console, you need to be authorized to access OBS.

#. Configure the following parameters:

   -  **Name**: **Flinktest**
   -  **Type**: **For general purpose**. Select **Dedicated Resource Mode**.
   -  **Specifications**: **16 CUs**
   -  **Enterprise Project**: **default**
   -  **Description**: Leave it blank.
   -  **Advanced Settings**: **Custom**
   -  **CIDR Block**: Set a CIDR block that does not conflict with the Kafka instance's CIDR block.

#. Click **Buy** and confirm the configuration.

#. Submit the request.

   It takes 10 to 15 minutes to bind the queue to a cluster after the queue is created.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section19012773105034:

Step 5: Create an Enhanced Datasource Connection Between DLI and Kafka
----------------------------------------------------------------------

You need to create an enhanced datasource connection for the Flink OpenSource SQL job. For details, see "Creating an Enhanced Datasource Connection".

.. note::

   -  The CIDR block of the DLI queue bound with a datasource connection cannot overlap with the CIDR block of the data source.
   -  Datasource connections cannot be created for the **default** queue.
   -  To access a table across data sources, you need to use a queue bound to a datasource connection.

#. .. _dli_01_0531__li13867111314415:

   Create a Kafka security group rule to allow access from the CIDR block of the DLI queue.

   a. On the Kafka management console, click an instance name on the **DMS for Kafka** page. Basic information of the Kafka instance is displayed.

   b. In the **Connection** pane, obtain the **Instance Address (Private Network)**. In the **Network** pane, obtain the VPC and subnet of the instance.

   c. Click the security group name in the **Network** pane. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from the DLI queue.

      For example, if the CIDR block of the queue is **10.0.0.0/16**, set **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

#. .. _dli_01_0531__li9182032194114:

   Create an enhanced datasource connection to Kafka.

   a. Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   b. In the displayed dialog box, set the following parameters: For details, see the following section:

      -  **Connection Name**: Name of the enhanced datasource connection For this example, enter **dli_kafka**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 4: Create a Queue <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`.
      -  **VPC**: Select the VPC of the Kafka instance.
      -  **Subnet**: Select the subnet of Kafka instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   c. Choose **Resources** > **Queue Management** and locate the queue created in :ref:`Step 4: Create a Queue <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`. In the **Operation** column, click **More** and select **Test Address Connectivity**.

   d. In the displayed dialog box, enter *Kafka instance address (private network)*\ **:**\ *port* in the **Address** box and click **Test** to check whether the instance is reachable. Note that multiple addresses must be tested separately.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section21433273112656:

Step 6: Create an Enhanced Datasource Connection Between DLI and RDS
--------------------------------------------------------------------

#. Create an RDS security group rule to allow access from CIDR block of the DLI queue.

   If the RDS DB instance and Kafka instance are in the same security group of the same VPC, skip this step. Access from the DLI queue has been allowed in :ref:`1 <dli_01_0531__li13867111314415>`.

   a. Go to the RDS console, click the name of the target RDS for MySQL DB instance on the **Instances** page. Basic information of the instance is displayed.
   b. In the **Connection Information** pane, obtain the floating IP address, database port, VPC, and subnet.
   c. Click the security group name. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from the DLI queue. For example, if the CIDR block of the queue is **10.0.0.0/16**, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

#. Create an enhanced datasource connection to RDS.

   If the RDS DB instance and Kafka instance are in the same VPC and subnet, skip this step. The enhanced datasource connection created in :ref:`2 <dli_01_0531__li9182032194114>` has connected the subnet.

   If the two instances are in different VPCs or subnets, perform the following steps to create an enhanced datasource connection:

   a. Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   b. In the displayed dialog box, set the following parameters: For details, see the following section:

      -  **Connection Name**: Name of the enhanced datasource connection For this example, enter **dli_rds**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 4: Create a Queue <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`.
      -  **VPC**: Select the VPC of the RDS DB instance.
      -  **Subnet**: Select the subnet of RDS DB instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   c. Choose **Resources** > **Queue Management** and locate the queue created in :ref:`Step 4: Create a Queue <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`. In the **Operation** column, click **More** and select **Test Address Connectivity**.

   d. In the displayed dialog box, enter *floating IP address*\ **:**\ *database port* of the RDS DB instance in the **Address** box and click **Test** to check whether the database is reachable.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section21590507141153:

Step 7: Create a Flink OpenSource SQL Job
-----------------------------------------

After the data source and data output channel are prepared, you can create a Flink OpenSource SQL job.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. In the upper right corner of the **Flink Jobs** page, click **Create Job**. Set the following parameters:

   -  **Type**: Flink OpenSource SQL
   -  **Name**: **JobSample**
   -  **Description**: Leave it blank.
   -  **Template Name**: Do not select any template.

#. Click **OK** to enter the editing page.

#. Set job running parameters. The mandatory parameters are as follows:

   -  **Queue**: **Flinktest**
   -  **Flink Version**: Select **1.12**.
   -  **Save Job Log**: Enable this function.
   -  **OBS Bucket**: Select an OBS bucket for storing job logs and grant access permissions of the OBS bucket as prompted.
   -  **Enable Checkpointing**: Enable this function.

   You do not need to set other parameters.

#. Click **Save**.

#. Edit the Flink OpenSource SQL job.

   In the SQL statement editing area, enter query and analysis statements as you need. The example statements are as follows. Note that the values of the parameters in bold must be changed according to the comments.

   .. code-block::

      CREATE TABLE kafkaSource (
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
        'connector' = 'kafka',
        'topic' = 'testkafkatopic',// Topic to be written to Kafka. Log in to the Kafka console, click the name of the created Kafka instance, and view the topic name on the Topic Management page.
        'properties.bootstrap.servers' = "192.168.0.237:9092,192.168.0.252:9092,192.168.0.137:9092", // Replace it with the internal network address and port number of Kafka.
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      CREATE TABLE jdbcSink (
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
        'connector' = 'jdbc',
        'url' = "jdbc:mysql://172.16.0.116:3306/rds-dliflink", //  testrdsdb indicates the name of the created RDS database. Replace the IP address and port number with those of the RDS for MySQL instance.
        'table-name' = 'orders',
        'pwd_auth_name'="xxxxx", // Name of the datasource authentication of the password type created on DLI. If datasource authentication is used, you do not need to set the username and password for the job.
        'sink.buffer-flush.max-rows' = '1'
      );

      insert into jdbcSink select * from kafkaSource;

#. Click **Check Semantics**.

#. Click **Start**. On the displayed **Start Flink Job** page, confirm the job specifications and the price, and click **Start Now** to start the job.

   After the job is started, the system automatically switches to the **Flink Jobs** page, and the created job is displayed in the job list. You can view the job status in the **Status** column. After a job is successfully submitted, **Status** of the job will change from **Submitting** to **Running**.

   If **Status** of a job is **Submission failed** or **Running exception**, the job fails to be submitted or fails to run. In this case, you can hover over the status icon to view the error details. You can click |image1| to copy these details. After handling the fault based on the provided information, resubmit the job.

#. Connect to the Kafka cluster and send the following test data to the Kafka topics:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

#. Run the following SQL statement in the MySQL database to view data in the table:

   .. code-block::

      select * from order;

   The following is an example of the execution result copied from the MySQL database:

   .. code-block::

      202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106
      202103241606060001,appShop,2021-03-24 16:06:06,200.0,180.0,2021-03-24 16:10:06,0001,Alice,330106

.. |image1| image:: /_static/images/en-us_image_0000001310151968.png
