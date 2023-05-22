:original_name: dli_03_0120.html

.. _dli_03_0120:

How Do I Change the Number of Kafka Partitions of a Flink SQL Job Without Stopping It?
======================================================================================

-  Symptom

   You used Flink 1.10 to run a Flink Opensource SQL job. You set the number of Kafka partitions for the job a small value at the beginning and need to increase the number now.

-  Solution

   Add the following parameters to the SQL statement:

   .. code-block::

      connector.properties.flink.partition-discovery.interval-millis="3000"

   This statement allows you to increase or decrease the number of Kafka partitions without stopping the Flink job.
