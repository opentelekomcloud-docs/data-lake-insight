:original_name: dli_09_0011.html

.. _dli_09_0011:

Reading Data from Kafka and Writing Data to Elasticsearch
=========================================================

.. important::

   This guide provides reference for Flink 1.12 only.

Description
-----------

This example analyzes offering purchase data and collects statistics on data results that meet specific conditions. The offering purchase data is stored in the Kafka source table, and then the analysis result is output to Elasticsearch .

For example, enter the following sample data:

.. code-block::

   {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

   {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0002", "user_name":"Jason", "area_id":"330106"}

DLI reads data from Kafka and writes the data to Elasticsearch. You can view the result in Kibana of the Elasticsearch cluster.

Prerequisites
-------------

#. You have created a DMS for Kafka instance.

   .. caution::

      When you create a DMS Kafka instance, do not enable **Kafka SASL_SSL**.

#. You have created a CSS Elasticsearch cluster.

   In this example, the version of the created CSS cluster is 7.6.2, and security mode is disabled for the cluster.

Overall Process
---------------

:ref:`Figure 1 <dli_09_0011__en-us_topic_0000001318422057_fig1691441652>` shows the overall development process.

.. _dli_09_0011__en-us_topic_0000001318422057_fig1691441652:

.. figure:: /_static/images/en-us_image_0000001318422061.png
   :alt: **Figure 1** Job development process

   **Figure 1** Job development process

:ref:`Step 1: Create a Queue <dli_09_0011__en-us_topic_0000001318422057_section792923214216>`

:ref:`Step 2: Create a Kafka Topic <dli_09_0011__en-us_topic_0000001318422057_section78516116518>`

:ref:`Step 3: Create an Elasticsearch Index <dli_09_0011__en-us_topic_0000001318422057_section1627154113018>`

:ref:`Step 4: Create an Enhanced Datasource Connection <dli_09_0011__en-us_topic_0000001318422057_section074025752119>`

:ref:`Step 5: Run a Job <dli_09_0011__en-us_topic_0000001318422057_section12448959174212>`

:ref:`Step 6: Send Data and Query Results <dli_09_0011__en-us_topic_0000001318422057_section4387527162418>`

.. _dli_09_0011__en-us_topic_0000001318422057_section792923214216:

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

.. _dli_09_0011__en-us_topic_0000001318422057_section78516116518:

Step 2: Create a Kafka Topic
----------------------------

#. On the Kafka management console, click an instance name on the **DMS for Kafka** page. Basic information of the Kafka instance is displayed.

#. Choose **Topics** in the navigation pane on the left. On the displayed page, click **Create Topic**. Configure the following parameters:

   -  **Topic Name**: For this example, enter **testkafkatopic**.
   -  **Partitions**: Set the value to **1**.
   -  **Replicas**: Set the value to **1**.

   Retain default values for other parameters.

.. _dli_09_0011__en-us_topic_0000001318422057_section1627154113018:

Step 3: Create an Elasticsearch Index
-------------------------------------

#. Log in to the CSS management console and choose **Clusters** > **Elasticsearch** from the navigation pane on the left.

#. On the **Clusters** page, click **Access Kibana** in the **Operation** column of the created CSS cluster.

#. On the displayed page, choose **Dev Tools** in the navigation pane on the left. The **Console** page is displayed.

#. On the displayed page, run the following command to create index **shoporders**:

   .. code-block:: text

      PUT /shoporders
      {
        "settings": {
          "number_of_shards": 1
        },
          "mappings": {
            "properties": {
              "order_id": {
                "type": "text"
              },
              "order_channel": {
                "type": "text"
              },
              "order_time": {
                "type": "text"
              },
              "pay_amount": {
                "type": "double"
              },
              "real_pay": {
                "type": "double"
              },
              "pay_time": {
                "type": "text"
              },
              "user_id": {
                "type": "text"
              },
              "user_name": {
                "type": "text"
              },
              "area_id": {
                "type": "text"
              }
            }
          }
      }

.. _dli_09_0011__en-us_topic_0000001318422057_section074025752119:

Step 4: Create an Enhanced Datasource Connection
------------------------------------------------

-  **Connecting DLI to Kafka**

   #. On the Kafka management console, click an instance name on the **DMS for Kafka** page. Basic information of the Kafka instance is displayed.

   #. In the **Connection** pane, obtain the **Instance Address (Private Network)**. In the **Network** pane, obtain the VPC and subnet of the instance.

   #. Click the security group name in the **Network** pane. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from DLI queues. For example, if the CIDR block of the queue is 10.0.0.0/16, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

   #. Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   #. In the displayed dialog box, set the following parameters: For details, see the following section:

      -  **Connection Name**: Enter a name for the enhanced datasource connection. For this example, enter **dli_kafka**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 1: Create a Queue <dli_09_0011__en-us_topic_0000001318422057_section792923214216>`.
      -  **VPC**: Select the VPC of the Kafka instance.
      -  **Subnet**: Select the subnet of Kafka instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   #. Choose **Resources** > **Queue Management** from the navigation pane, locate the queue you created in :ref:`Step 1: Create a Queue <dli_09_0011__en-us_topic_0000001318422057_section792923214216>`. In the **Operation** column, click **More** > **Test Address Connectivity**.

   #. In the displayed dialog box, enter *Kafka instance address (private network)*\ **:**\ *port* in the **Address** box and click **Test** to check whether the instance is reachable.

-  **Connecting DLI to CSS**

   #. On the CSS management console, choose **Clusters** > **Elasticsearch**. On the displayed page, click the name of the created CSS cluster to view basic information.

   #. .. _dli_09_0011__en-us_topic_0000001318422057_li19666016361:

      On the **Cluster Information** page, obtain the **Private Network Address**, **VPC**, AND **Subnet**.

   #. Click the security group name. On the displayed page, click the **Inbound Rules** tab and add a rule to allow access from DLI queues. For example, if the CIDR block of the queue is 10.0.0.0/16, set **Priority** to **1**, **Action** to **Allow**, **Protocol** to **TCP**, **Type** to **IPv4**, **Source** to **10.0.0.0/16**, and click **OK**.

   #. Check whether the Kafka instance and Elasticsearch instance are in the same VPC and subnet.

      a. If they are, go to :ref:`7 <dli_09_0011__en-us_topic_0000001318422057_li9816175412318>`. You do not need to create an enhanced datasource connection again.
      b. If they are not, go to :ref:`5 <dli_09_0011__en-us_topic_0000001318422057_li11976319011>`. Create an enhanced datasource connection to connect DLI to the subnet where the Elasticsearch instance locates.

   #. .. _dli_09_0011__en-us_topic_0000001318422057_li11976319011:

      Log in to the DLI management console. In the navigation pane on the left, choose **Datasource Connections**. On the displayed page, click **Create** in the **Enhanced** tab.

   #. In the displayed dialog box, set the following parameters: For details, see the following section:

      -  **Connection Name**: Enter a name for the enhanced datasource connection. For this example, enter **dli_css**.
      -  **Resource Pool**: Select the name of the queue created in :ref:`Step 1: Create a Queue <dli_09_0011__en-us_topic_0000001318422057_section792923214216>`.
      -  **VPC**: Select the VPC of the Elasticsearch instance.
      -  **Subnet**: Select the subnet of Elasticsearch instance.
      -  Set other parameters as you need.

      Click **OK**. Click the name of the created datasource connection to view its status. You can perform subsequent steps only after the connection status changes to **Active**.

   #. .. _dli_09_0011__en-us_topic_0000001318422057_li9816175412318:

      Choose **Resources** > **Queue Management** from the navigation pane, locate the queue you created in :ref:`Step 1: Create a Queue <dli_09_0011__en-us_topic_0000001318422057_section792923214216>`. In the **Operation** column, click **More** > **Test Address Connectivity**.

   #. In the displayed dialog box, enter *floating IP address*\ **:**\ *database port* of the Elasticsearch instance you have obtained in :ref:`2 <dli_09_0011__en-us_topic_0000001318422057_li19666016361>` in the **Address** box and click **Test** to check whether the database is reachable.

.. _dli_09_0011__en-us_topic_0000001318422057_section12448959174212:

Step 5: Run a Job
-----------------

#. On the DLI management console, choose **Job Management** > **Flink Jobs**. On the **Flink Jobs** page, click **Create Job**.
#. In the the **Create Job** dialog box, set **Type** to **Flink OpenSource SQL** and **Name** to **FlinkKafkaES**. Click **OK**.
#. On the job editing page, set the following parameters and retain the default values of other parameters.

   -  **Queue**: Select the queue created in :ref:`Step 1: Create a Queue <dli_09_0011__en-us_topic_0000001318422057_section792923214216>`.

   -  **Flink Version**: Select **1.12**.

   -  **Save Job Log**: Enable this function.

   -  **OBS Bucket**: Select an OBS bucket for storing job logs and grant access permissions of the OBS bucket as prompted.

   -  **Enable Checkpointing**: Enable this function.

   -  Enter a SQL statement in the editing pane. The following is an example. Modify the parameters in bold as you need.

      .. note::

         In this example, the syntax version of Flink OpenSource SQL is 1.12. In this example, the data source is Kafka and the result data is written to Elasticsearch.

   -  Create a Kafka source table and connect DLI to the Kafka data source.

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
         ) with (
           "connector" = "kafka",
           "properties.bootstrap.servers" = "10.128.0.120:9092,10.128.0.89:9092,10.128.0.83:9092",-- Internal network address and port number of the Kafka instance
           "properties.group.id" = "click",
           "topic" = "testkafkatopic",--Created Kafka topic
           "format" = "json",
           "scan.startup.mode" = "latest-offset"
         );

   -  Create an Elasticsearch result table to display the data analyzed by DLI.

      .. code-block::

         CREATE TABLE elasticsearchSink (
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
           'connector' = 'elasticsearch-7',
           'hosts' = '192.168.168.125:9200', --Private IP address and port of the CSS cluster
           'index' = 'shoporders' --Created Elasticsearch engine
         );
         --Write Kafka data to Elasticsearch indexes
         insert into
           elasticsearchSink
         select
           *
         from
           kafkaSource;

#. Click **Check Semantic** and ensure that the SQL statement passes the check. Click **Save**. Click **Start**, confirm the job parameters, and click **Start Now** to execute the job. Wait until the job status changes to **Running**.

.. _dli_09_0011__en-us_topic_0000001318422057_section4387527162418:

Step 6: Send Data and Query Results
-----------------------------------

#. Kafaka sends data.

   Use the Kafka client to send data to topics created in :ref:`Step 2: Create a Kafka Topic <dli_09_0011__en-us_topic_0000001318422057_section78516116518>` to simulate real-time data streams.

   The sample data is as follows:

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241606060001", "order_channel":"appShop", "order_time":"2021-03-24 16:06:06", "pay_amount":"200.00", "real_pay":"180.00", "pay_time":"2021-03-24 16:10:06", "user_id":"0002", "user_name":"Jason", "area_id":"330106"}

#. View the data processing result on Elasticsearch.

   After the message is sent to Kafka, run the following statement in Kibana for the CSS cluster and check the result:

   .. code-block:: text

      GET shoporders/_search

   The query result is as follows:

   .. code-block::

      {
        "took" : 0,
        "timed_out" : false,
        "_shards" : {
          "total" : 1,
          "successful" : 1,
          "skipped" : 0,
          "failed" : 0
        },
        "hits" : {
          "total" : {
            "value" : 2,
            "relation" : "eq"
          },
          "max_score" : 1.0,
          "hits" : [
            {
              "_index" : "shoporders",
              "_type" : "_doc",
              "_id" : "6fswzIAByVjqg3_qAyM1",
              "_score" : 1.0,
              "_source" : {
                "order_id" : "202103241000000001",
                "order_channel" : "webShop",
                "order_time" : "2021-03-24 10:00:00",
                "pay_amount" : 100.0,
                "real_pay" : 100.0,
                "pay_time" : "2021-03-24 10:02:03",
                "user_id" : "0001",
                "user_name" : "Alice",
                "area_id" : "330106"
              }
            },
            {
              "_index" : "shoporders",
              "_type" : "_doc",
              "_id" : "6vs1zIAByVjqg3_qyyPp",
              "_score" : 1.0,
              "_source" : {
                "order_id" : "202103241606060001",
                "order_channel" : "appShop",
                "order_time" : "2021-03-24 16:06:06",
                "pay_amount" : 200.0,
                "real_pay" : 180.0,
                "pay_time" : "2021-03-24 16:10:06",
                "user_id" : "0002",
                "user_name" : "Jason",
                "area_id" : "330106"
              }
            }
          ]
        }
      }
