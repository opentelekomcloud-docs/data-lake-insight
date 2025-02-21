:original_name: dli_01_0531.html

.. _dli_01_0531:

Using DLI to Submit a Flink OpenSource SQL Job to Query RDS for MySQL Data
==========================================================================

Scenario
--------

DLI Flink jobs can use other cloud services as data sources and sink streams for real-time compute.

This example describes how to create and submit a Flink OpenSource SQL job that uses Kafka as the source stream and RDS as the sink stream.

.. _dli_01_0531__section7285125165914:

Procedure
---------

You need to create a Flink OpenSource SQL job that has a source stream and a sink stream. The source stream reads data from Kafka, and the sink stream writes data to RDS for MySQL. :ref:`Procedure <dli_01_0531__section7285125165914>` shows the process.

.. table:: **Table 1** Procedure for using DLI to submit a Flink OpenSource SQL job to query RDS for MySQL data

   +-------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | Procedure                                                                                                                                                   | Description                                                                                                              |
   +=============================================================================================================================================================+==========================================================================================================================+
   | :ref:`Step 1: Prepare a Source Stream <dli_01_0531__en-us_topic_0000001354966081_section1133722963618>`                                                     | In this example, a Kafka instance is created as the data source.                                                         |
   +-------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 2: Prepare a Sink Stream <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section10891114913473>`                                          | In this example, an RDS for MySQL DB instance is created as the data destination.                                        |
   +-------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 3: Create an OBS Bucket to Store Output Data <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section18912701608>`                         | Create an OBS bucket to store checkpoints, job logs, and debugging test data for the Flink OpenSource SQL job.           |
   +-------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 4: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`    | Create compute resources required for submitting the Flink OpenSource SQL job.                                           |
   +-------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 5: Create an Enhanced Datasource Connection Between DLI and Kafka <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section19012773105034>` | Create an enhanced datasource connection to connect the DLI elastic resource pool and the Kafka instance.                |
   +-------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 6: Create an Enhanced Datasource Connection Between DLI and RDS <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section21433273112656>`   | Create an enhanced datasource connection to connect the DLI elastic resource pool and the RDS for MySQL DB instance.     |
   +-------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`Step 7: Create a Flink OpenSource SQL Job <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section21590507141153>`                              | Once you have prepared a source stream and a sink stream, you can create a Flink OpenSource SQL job to analyze the data. |
   +-------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+

.. _dli_01_0531__en-us_topic_0000001354966081_section1133722963618:

Step 1: Prepare a Source Stream
-------------------------------

In this example, Kafka is the source stream.

Enable DIS to import Kafka data to DLI. For details, see "Buying a Kafka Instance" in the *Distributed Message Service Kafka User Guide*.

#. .. _dli_01_0531__en-us_topic_0000001354966081_li485218325375:

   Create the dependent Kafka resources.

   Before creating a Kafka instance, ensure the availability of resources, including a virtual private cloud (VPC), subnet, security group, and security group rules.

   -  For how to create a VPC and subnet, see "Creating a VPC and Subnet" in the *Virtual Private Cloud User Guide*. For how to create and use a subnet in an existing VPC, see "Create a Subnet for the VPC" in the *Virtual Private Cloud User Guide*.

      .. note::

         -  The created VPC and the Kafka instance you will create must be in the same region.
         -  Retain the default settings unless otherwise specified.

   -  For how to create a security group, see "Creating a Security Group" in the *Virtual Private Cloud User Guide*. For how to add rules to a security group, see "Creating a Subnet for the VPC" in the *Virtual Private Cloud User Guide*.

#. Create a Kafka premium instance as the job source stream.

   a. Log in to the DMS for Kafka management console.
   b. Select a region in the upper left corner.
   c. On the **DMS for Kafka** page, click **Buy Instance** in the upper right corner and set related parameters. The required instance information is as follows:

      .. table:: **Table 2** Parameters for buying a DMS for Kafka instance

         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Parameter                | Description                                                                                                                                                                              | Example Value                                                                                             |
         +==========================+==========================================================================================================================================================================================+===========================================================================================================+
         | Region                   | DMS for Kafka instances in different regions cannot communicate with each other over an intranet. For lower network latency and quick resource access, select the region nearest to you. | \_                                                                                                        |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | AZ                       | An AZ is a physical location that uses an independent power supply and network. AZs within the same region can communicate with each other over an intranet.                             | AZ 1, AZ 2, and AZ 3                                                                                      |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Version                  | Kafka version, which cannot be changed after the instance is created.                                                                                                                    | 3.x                                                                                                       |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Specifications           | The options are **Cluster** and **Single-node**.                                                                                                                                         | Cluster                                                                                                   |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Broker Flavor            | Select a broker flavor based on service requirements.                                                                                                                                    | kafka.2u4g.cluster.small                                                                                  |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Brokers                  | Specify the broker quantity.                                                                                                                                                             | 3                                                                                                         |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Storage Space per Broker | Select a disk type and specify the disk size. You cannot change the disk type once the instance is created.                                                                              | High I/O \| 100 GB                                                                                        |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | VPC                      | Select a created or shared VPC.                                                                                                                                                          | Select the VPC created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`.            |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Subnet                   | Select a created or shared subnet.                                                                                                                                                       | Select the subnet created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`.         |
         |                          |                                                                                                                                                                                          |                                                                                                           |
         |                          | Once the instance is created, its subnet cannot be changed.                                                                                                                              |                                                                                                           |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Security Group           | Select a created security group.                                                                                                                                                         | Select the security group created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`. |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Access Mode              | There are two modes:                                                                                                                                                                     | Plaintext Access                                                                                          |
         |                          |                                                                                                                                                                                          |                                                                                                           |
         |                          | -  **Plaintext Access**: SASL authentication is not conducted when a client connects to the Kafka instance.                                                                              |                                                                                                           |
         |                          | -  **Ciphertext Access**: SASL authentication is conducted when a client connects to the Kafka instance.                                                                                 |                                                                                                           |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Instance Name            | Enter an instance name based on the naming rule.                                                                                                                                         | kafka-dliflink                                                                                            |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
         | Enterprise Project       | An enterprise project organizes cloud resources into groups, allowing you to manage both resources and members by project. The default project is **default**.                           | default                                                                                                   |
         +--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+

   d. Click **Buy**.
   e. Confirm that the instance information is correct, read and agree to the , and click **Submit**. It takes about 10 to 15 minutes to create an instance.

#. Create a Kafka topic.

   a. Click the name of the created Kafka instance. The basic information page of the instance is displayed.

   b. Choose **Topics** in the navigation pane on the left. On the displayed page, click **Create Topic**. Configure the following parameters:

      -  **Topic Name**: For this example, enter **testkafkatopic**.
      -  **Partitions**: Set the value to **1**.
      -  **Replicas**: Set the value to **1**.

      Retain the default values for other parameters.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section10891114913473:

Step 2: Prepare a Sink Stream
-----------------------------

Use RDS for MySQL as the data sink stream and create an RDS for MySQL DB instance.

#. Log in to the RDS management console.

#. Select a region in the upper left corner.

#. Click **Buy DB Instance** in the upper right corner of the page and set related parameters. Retain the default values for other parameters.

   For the parameters, see "RDS for MySQL Getting Started" in the *Relational Database Service Getting Started*.

   .. table:: **Table 3** RDS for MySQL instance parameters

      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Parameter              | Description                                                                                                                                                                                                                           | Example Value                                                                                             |
      +========================+=======================================================================================================================================================================================================================================+===========================================================================================================+
      | Region                 | Select the region where DLI is.                                                                                                                                                                                                       | \_                                                                                                        |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Instance Name          | Instance name                                                                                                                                                                                                                         | rds-dliflink                                                                                              |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | DB Engine              | MySQL                                                                                                                                                                                                                                 | MySQL                                                                                                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | DB Engine Version      | If you select **MySQL** for **DB Engine**, select an engine version that best suits your service needs. You are advised to select the latest available version for more stable performance, higher security, and greater reliability. | 8.0                                                                                                       |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | DB Instance Type       | Primary/standby mode of the DB instance                                                                                                                                                                                               | Primary/Standby                                                                                           |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Storage Type           | Determines the instance read/write speed. The higher the maximum throughput, the faster the read and write speeds.                                                                                                                    | Cloud SSD                                                                                                 |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | AZ                     | For a single DB instance, you only need to select a single AZ.                                                                                                                                                                        | Custom                                                                                                    |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Time Zone              | Select a time zone based on the region you selected. You can change it after the DB instance is created.                                                                                                                              | Retain the default value.                                                                                 |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Instance Class         | vCPUs and memory. These instance classes support varying number of connections and maximum IOPS.                                                                                                                                      | 2 vCPUs \| 4 GB                                                                                           |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Storage Space          | If the storage type is cloud SSD or extreme SSD, you can enable storage autoscaling. If the available storage drops to a specified threshold, autoscaling is triggered.                                                               | 40 GB                                                                                                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Disk Encryption        | Determine whether to enable disk encryption.                                                                                                                                                                                          | Disabled                                                                                                  |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | VPC and Subnet         | Select an existing VPC and subnet.                                                                                                                                                                                                    | Select the VPC and subnet created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`. |
      |                        |                                                                                                                                                                                                                                       |                                                                                                           |
      |                        | For how to recreate a VPC and subnet, refer to "Creating a VPC and Subnet" in the *Virtual Private Cloud User Guide*.                                                                                                                 |                                                                                                           |
      |                        |                                                                                                                                                                                                                                       |                                                                                                           |
      |                        | .. note::                                                                                                                                                                                                                             |                                                                                                           |
      |                        |                                                                                                                                                                                                                                       |                                                                                                           |
      |                        |    In datasource scenarios, the CIDR block of the data source cannot overlap that of the elastic resource pool.                                                                                                                       |                                                                                                           |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Database Port          | Port **3306** is used by default.                                                                                                                                                                                                     | 3306                                                                                                      |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Security Group         | Enhances security by providing rules that control access to RDS from other services.                                                                                                                                                  | Select the security group created in :ref:`1 <dli_01_0531__en-us_topic_0000001354966081_li485218325375>`. |
      |                        |                                                                                                                                                                                                                                       |                                                                                                           |
      |                        | The security group where the data source is must allow access from the CIDR block of the DLI elastic resource pool.                                                                                                                   |                                                                                                           |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Password               | Set a password for logging in to the DB instance.                                                                                                                                                                                     | ``-``                                                                                                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Administrator          | root                                                                                                                                                                                                                                  | root                                                                                                      |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Administrator Password | Administrator password                                                                                                                                                                                                                | ``-``                                                                                                     |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Parameter Template     | A template of parameters for creating an instance. The template contains engine configuration values that are applied to one or more instances.                                                                                       | Default-MySQL-8.0                                                                                         |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Enterprise Project     | If the instance has been associated with an enterprise project, select the target project from the **Enterprise Project** drop-down list.                                                                                             | default                                                                                                   |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
      | Quantity               | Number of instances to buy                                                                                                                                                                                                            | 1                                                                                                         |
      +------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------+

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

In this example, you need to enable OBS for job **JobSample** to provide DLI Flink jobs with the functions of checkpointing, saving job logs, and commissioning test data.

For how to create a bucket, see "Creating a Bucket" in the *Object Storage Service Console Operation Guide*.

#. In the navigation pane on the OBS management console, choose **Object Storage**.
#. In the upper right corner of the page, click **Create Bucket** and set bucket parameters.

   .. table:: **Table 4** OBS bucket parameters

      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                        | Example Value         |
      +=======================+====================================================================================================================================+=======================+
      | Region                | Geographic area where a bucket resides. Select the region where DLI is.                                                            | \_                    |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Bucket Name           | Name of the bucket. The name must be unique across all regions and accounts. Once a bucket is created, its name cannot be changed. | obstest               |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Storage Class         | Storage class of the bucket. You can choose a storage class that meets your needs for storage performance and costs.               | Standard              |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Bucket Policies       | Controls read and write permissions for a bucket.                                                                                  | Private               |
      |                       |                                                                                                                                    |                       |
      |                       | If you select **Private**, only users granted permissions by the bucket ACL can access the bucket.                                 |                       |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Enterprise Project    | You can add the bucket to an enterprise project for unified management.                                                            | default               |
      +-----------------------+------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **Create Now**.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710:

Step 4: Create an Elastic Resource Pool and Add Queues to the Pool
------------------------------------------------------------------

To create a Flink OpenSource SQL job, you must use your own queue as the existing **default** queue cannot be used. In this example, create an elastic resource pool named **dli_resource_pool** and a queue named **dli_queue_01**.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Resources** > **Resource Pool**.

#. On the displayed page, click **Buy Resource Pool** in the upper right corner.

#. On the displayed page, set the parameters.

   :ref:`Table 5 <dli_01_0531__dli_01_0002_table67098261452>` describes the parameters.

   .. _dli_01_0531__dli_01_0002_table67098261452:

   .. table:: **Table 5** Parameters

      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Parameter          | Description                                                                                                                                                                                             | Example Value     |
      +====================+=========================================================================================================================================================================================================+===================+
      | Region             | Select a region where you want to buy the elastic resource pool.                                                                                                                                        | \_                |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Project            | Project uniquely preset by the system for each region                                                                                                                                                   | Default           |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Name               | Name of the elastic resource pool                                                                                                                                                                       | dli_resource_pool |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Specifications     | Specifications of the elastic resource pool                                                                                                                                                             | Standard          |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | CU Range           | The maximum and minimum CUs allowed for the elastic resource pool                                                                                                                                       | 64-64             |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | CIDR Block         | CIDR block the elastic resource pool belongs to. If you use an enhanced datasource connection, this CIDR block cannot overlap that of the data source. **Once set, this CIDR block cannot be changed.** | 172.16.0.0/19     |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+
      | Enterprise Project | Select an enterprise project for the elastic resource pool.                                                                                                                                             | default           |
      +--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------+

#. Click **Buy**.

#. Click **Submit**.

#. In the elastic resource pool list, locate the pool you just created and click **Add Queue** in the **Operation** column.

#. Set the basic parameters listed below.

   .. table:: **Table 6** Basic parameters for adding a queue

      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                        | Example Value         |
      +=======================+====================================================================+=======================+
      | Name                  | Name of the queue to add                                           | dli_queue_01          |
      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Type                  | Type of the queue                                                  | \_                    |
      |                       |                                                                    |                       |
      |                       | -  To execute SQL jobs, select **For SQL**.                        |                       |
      |                       | -  To execute Flink or Spark jobs, select **For general purpose**. |                       |
      +-----------------------+--------------------------------------------------------------------+-----------------------+
      | Enterprise Project    | Select an enterprise project.                                      | default               |
      +-----------------------+--------------------------------------------------------------------+-----------------------+

#. Click **Next** and configure scaling policies for the queue.

   Click **Create** to add a scaling policy with varying priority, period, minimum CUs, and maximum CUs.

   .. table:: **Table 7** Scaling policy parameters

      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                                                          | Example Value         |
      +=======================+======================================================================================================================================================================================================================+=======================+
      | Priority              | Priority of the scaling policy in the current elastic resource pool. A larger value indicates a higher priority. In this example, only one scaling policy is configured, so its priority is set to **1** by default. | 1                     |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Period                | The first scaling policy is the default policy, and its **Period** parameter configuration cannot be deleted or modified.                                                                                            | 00-24                 |
      |                       |                                                                                                                                                                                                                      |                       |
      |                       | The period for the scaling policy is from 00 to 24.                                                                                                                                                                  |                       |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Min CU                | Minimum number of CUs allowed by the scaling policy                                                                                                                                                                  | 16                    |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Max CU                | Maximum number of CUs allowed by the scaling policy                                                                                                                                                                  | 64                    |
      +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Click **OK**.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section19012773105034:

Step 5: Create an Enhanced Datasource Connection Between DLI and Kafka
----------------------------------------------------------------------

You need to create an enhanced datasource connection for the Flink OpenSource SQL job. For details, see "Datasource Connections" > "Creating an Enhanced Datasource Connection" in the *Data Lake Insight User Guide*.

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
      -  **Resource Pool**: Select the elastic resource pool created in :ref:`Step 4: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`.
      -  **VPC**: Select the VPC of the Kafka instance.
      -  **Subnet**: Select the subnet of Kafka instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   c. Choose **Resources** > **Queue Management** and locate the queue created in :ref:`Step 4: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`. In the **Operation** column, click **More** and select **Test Address Connectivity**.

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
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 4: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`.
      -  **VPC**: Select the VPC of the RDS DB instance.
      -  **Subnet**: Select the subnet of RDS DB instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   c. Choose **Resources** > **Queue Management** and locate the queue created in :ref:`Step 4: Create an Elastic Resource Pool and Add Queues to the Pool <dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section122981023152710>`. In the **Operation** column, click **More** and select **Test Address Connectivity**.

   d. In the displayed dialog box, enter *Floating IP address*\ **:**\ *Database port* of the RDS for MySQL DB instance in the **Address** box and click **Test** to check if the instance is reachable.

.. _dli_01_0531__en-us_topic_0000001354966081_dli_01_0481_section21590507141153:

Step 7: Create a Flink OpenSource SQL Job
-----------------------------------------

After the source and sink streams are prepared, you can create a Flink OpenSource SQL job.

#. In the left navigation pane of the DLI management console, choose **Job Management** > **Flink Jobs**. The **Flink Jobs** page is displayed.

#. In the upper right corner of the **Flink Jobs** page, click **Create Job**. Set the following parameters:

   -  **Type**: Flink OpenSource SQL
   -  **Name**: **JobSample**
   -  **Description**: Leave it blank.
   -  **Template Name**: Do not select any template.

#. Click **OK** to enter the editing page.

#. Set job running parameters. The mandatory parameters are as follows:

   -  **Queue**: **dli_queue_01**
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

   If **Status** of a job is **Submission failed** or **Running exception**, the job fails to be submitted or fails to run. In this case, you can hover over the status icon to view the error details. You can click |image1| to copy these details. Rectify the fault based on the error information and resubmit the job.

#. Connect to the Kafka cluster and send the following test data to the Kafka topics:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

#. Run the following SQL statement in the MySQL database to view data in the table:

   .. code-block::

      select * from orders;

   The following is an example of the execution result copied from the MySQL database:

   .. code-block::

      202103241000000001,webShop,2021-03-24 10:00:00,100.0,100.0,2021-03-24 10:02:03,0001,Alice,330106
      202103241606060001,appShop,2021-03-24 16:06:06,200.0,180.0,2021-03-24 16:10:06,0001,Alice,330106

.. |image1| image:: /_static/images/en-us_image_0000001310151968.png
