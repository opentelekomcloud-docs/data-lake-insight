:original_name: dli_08_0438.html

.. _dli_08_0438:

string_split
============

The **string_split** function splits a target string into substrings based on the specified separator and returns a substring list.

Description
-----------

.. code-block::

   string_split(target, separator)

.. table:: **Table 1** string_split parameters

   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Data Types            | Description                                                                                                       |
   +=======================+=======================+===================================================================================================================+
   | target                | STRING                | Target string to be processed                                                                                     |
   |                       |                       |                                                                                                                   |
   |                       |                       | .. note::                                                                                                         |
   |                       |                       |                                                                                                                   |
   |                       |                       |    -  If **target** is **NULL**, an empty line is returned.                                                       |
   |                       |                       |    -  If **target** contains two or more consecutive separators, an empty substring is returned.                  |
   |                       |                       |    -  If **target** does not contain a specified separator, the original string passed to **target** is returned. |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------+
   | separator             | VARCHAR               | Separator. Currently, only single-character separators are supported.                                             |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------+

Example
-------

#. Create a Flink OpenSource SQL job by referring to :ref:`Kafka Source Table <dli_08_0386>` and :ref:`Print Result Table <dli_08_0399>`, enter the following job running script, and submit the job.

   When you create a job, set **Flink Version** to **1.12** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. **Change the values of the parameters in bold as needed in the following script.**

   .. code-block::

      CREATE TABLE kafkaSource (
        target STRING,
        separator  VARCHAR
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'KafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        "format" = "json"
      );

      CREATE TABLE printSink (
        target STRING,
        item STRING
      ) WITH (
        'connector' = 'print'
      );

      insert into printSink
        select target,
        item from
        kafkaSource,
        lateral table(string_split(target, separator)) as T(item);

#. Connect to the Kafka cluster and send the following test data to the Kafka topic:

   .. code-block::

      {"target":"test-flink","separator":"-"}
      {"target":"flink","separator":"-"}
      {"target":"one-two-ww-three","separator":"-"}

   The data is as follows:

   .. table:: **Table 2** Test table data

      ================ ===================
      target (STRING)  separator (VARCHAR)
      ================ ===================
      test-flink       ``-``
      flink            ``-``
      one-two-ww-three ``-``
      ================ ===================

#. View output.

   -  Method 1:

      a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
      b. Locate the row that contains the target Flink job, and choose **More** > **FlinkUI** in the **Operation** column.
      c. On the Flink UI, choose **Task Managers**, click the task name, and select **Stdout** to view job logs.

   -  Method 2: If you select **Save Job Log** on the **Running Parameters** tab before submitting the job, perform the following operations:

      a. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
      b. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
      c. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

   The query result is as follows:

   .. code-block::

      +I(test-flink,test)
      +I(test-flink,flink)
      +I(flink,flink)
      +I(one-two-ww-three,one)
      +I(one-two-ww-three,two)
      +I(one-two-ww-three,ww)
      +I(one-two-ww-three,three)

   The output data is as follows:

   .. table:: **Table 3** Result table data

      ================ =============
      target (STRING)  item (STRING)
      ================ =============
      test-flink       test
      test-flink       flink
      flink            flink
      one-two-ww-three one
      one-two-ww-three two
      one-two-ww-three ww
      one-two-ww-three three
      ================ =============
