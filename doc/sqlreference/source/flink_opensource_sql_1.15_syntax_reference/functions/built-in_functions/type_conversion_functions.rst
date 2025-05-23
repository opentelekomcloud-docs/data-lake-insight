:original_name: dli_08_15092.html

.. _dli_08_15092:

Type Conversion Functions
=========================

.. table:: **Table 1** Type conversion functions

   +----------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SQL Function                                       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                      |
   +====================================================+==================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | CAST(value AS type)                                | Returns a new value that has been converted to the type.                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                                                    | For example, **CAST('42' AS INT)** returns **42**;                                                                                                                                                                                                                                                                                                                                                                                               |
   |                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                                                    | **CAST(NULL AS VARCHAR)** returns **NULL** of type **VARCHAR**.                                                                                                                                                                                                                                                                                                                                                                                  |
   +----------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TYPEOF(input) \| TYPEOF(input, force_serializable) | Returns a string representation of the data type of the input expression. By default, the returned string is a summary string that may omit some details for readability. If **force_serializable** is set to **TRUE**, the string representation can preserve the full data type that is stored in the catalog. Note that anonymous inline data types do not have a serializable string representation, and in this case, **NULL** is returned. |
   +----------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

CAST Syntax Format
------------------

.. code-block::

   CAST(value AS type)

CAST Syntax Description
-----------------------

This syntax is used to forcibly convert types.

CAST Caveats
------------

If the input is **NULL**, **NULL** is returned.

CAST Example 1: Converting the Amount Value to an Integer
---------------------------------------------------------

The following example converts the **amount** value to an integer.

.. code-block::

   insert into temp select cast(amount as INT) from source_stream;

.. table:: **Table 2** Examples of CAST type conversion functions

   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+
   | Example               | Description                                                                                                          | Example                            |
   +=======================+======================================================================================================================+====================================+
   | cast(v1 as string)    | Converts **v1** to a string. The value of **v1** can be of the numeric type or of the timestamp, date, or time type. | Table T1:                          |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    | content (INT)           |     |
   |                       |                                                                                                                      |    | -------------           |     |
   |                       |                                                                                                                      |    | 5                       |     |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Statement:                         |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    SELECT                          |
   |                       |                                                                                                                      |      cast(content as varchar)      |
   |                       |                                                                                                                      |    FROM                            |
   |                       |                                                                                                                      |      T1;                           |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Result:                            |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    "5"                             |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+
   | cast (v1 as int)      | Converts **v1** to the **int** type. The value of **v1** can be a number or a character.                             | Table T1:                          |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    | content  (STRING)           | |
   |                       |                                                                                                                      |    | -------------               | |
   |                       |                                                                                                                      |    | "5"                         | |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Statement:                         |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    SELECT                          |
   |                       |                                                                                                                      |      cast(content as int)          |
   |                       |                                                                                                                      |    FROM                            |
   |                       |                                                                                                                      |      T1;                           |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Result:                            |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    5                               |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+
   | cast(v1 as timestamp) | Converts **v1** to the **timestamp** type. The value of **v1** can be of the **string**, **date**, or **time** type. | Table T1:                          |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    | content  (STRING)          |  |
   |                       |                                                                                                                      |    | -------------              |  |
   |                       |                                                                                                                      |    | "2018-01-01 00:00:01"     |   |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Statement:                         |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    SELECT                          |
   |                       |                                                                                                                      |      cast(content as timestamp)    |
   |                       |                                                                                                                      |    FROM                            |
   |                       |                                                                                                                      |      T1;                           |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Result:                            |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    1514736001000                   |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+
   | cast(v1 as date)      | Converts **v1** to the **date** type. The value of **v1** can be of the **string** or **timestamp** type.            | Table T1:                          |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    | content  (TIMESTAMP)     |    |
   |                       |                                                                                                                      |    | -------------            |    |
   |                       |                                                                                                                      |    | 1514736001000            |    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Statement:                         |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    SELECT                          |
   |                       |                                                                                                                      |      cast(content as date)         |
   |                       |                                                                                                                      |    FROM                            |
   |                       |                                                                                                                      |      T1;                           |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | Result:                            |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      | .. code-block::                    |
   |                       |                                                                                                                      |                                    |
   |                       |                                                                                                                      |    "2018-01-01"                    |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------+------------------------------------+

.. note::

   Flink jobs do not support the conversion of **bigint** to **timestamp** using CAST. You can convert it using **to_timestamp**.

CAST Example 2
--------------

#. Create a Flink OpenSource SQL job by referring to :ref:`Kafka <dli_08_15058>` and :ref:`Print <dli_08_15060>`, enter the following job running script, and submit the job.

   When you create a job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs. Change the values of the parameters in bold in the following script according to the actual situation.

   .. code-block::

      CREATE TABLE kafkaSource (
        cast_int_to_string int,
        cast_String_to_int string,
        case_string_to_timestamp string,
        case_timestamp_to_date timestamp
      ) WITH (
        'connector' = 'kafka',
        'topic' = 'KafkaTopic',
        'properties.bootstrap.servers' = 'KafkaAddress1:KafkaPort,KafkaAddress2:KafkaPort',
        'properties.group.id' = 'GroupId',
        'scan.startup.mode' = 'latest-offset',
        "format" = "json"
      );

      CREATE TABLE printSink (
        cast_int_to_string string,
        cast_String_to_int int,
        case_string_to_timestamp timestamp,
        case_timestamp_to_date date
      ) WITH (
        'connector' = 'print'
      );

      insert into printSink select
        cast(cast_int_to_string as string),
        cast(cast_String_to_int as int),
        cast(case_string_to_timestamp as timestamp),
        cast(case_timestamp_to_date as date)
      from kafkaSource;

#. Connect to the Kafka cluster and send the following test data to the Kafka topic:

   .. code-block::

      {"cast_int_to_string":"1", "cast_String_to_int": "1", "case_string_to_timestamp": "2022-04-02 15:00:00", "case_timestamp_to_date": "2022-04-02 15:00:00"}

#. View output.

   -  Method 1:

      a. Log in to the DLI management console and choose Job Management > Flink Streaming Jobs.
      b. Locate the row that contains the target Flink job, and choose More & > FlinkUI in the Operation column.
      c. On the Flink UI, choose Task Managers, click the task name, and select Stdout to view the job run logs.

   -  Method 2: If you select **Save Job Log** on the **Running Parameters** tab before submitting the job, perform the following operations:

      a. Log in to the DLI management console and choose Job Management > Flink Streaming Jobs.
      b. Click the name of the corresponding Flink job, choose Run Log, click OBS Bucket, and locate the folder of the corresponding log based on the job running date.
      c. Go to the folder of the corresponding date, find the folder whose name contains taskmanager, download the taskmanager.out file, and view the result log.

   The query result is as follows:

   .. code-block::

      +I(1,1,2022-04-02T15:00,2022-04-02)
