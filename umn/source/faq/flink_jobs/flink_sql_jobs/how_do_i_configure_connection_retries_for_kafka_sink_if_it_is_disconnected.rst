:original_name: dli_03_0121.html

.. _dli_03_0121:

How Do I Configure Connection Retries for Kafka Sink If it is Disconnected?
===========================================================================

Symptom
-------

You used Flink 1.10 to run a Flink Opensource SQL job. The job failed after the following error was reported when Flink Sink wrote data to Kafka.

.. code-block::

   Caused by: org.apache.kafka.common.errors.NetworkException: The server disconnected before a response was received.

Possible Causes
---------------

The CPU usage is too high. As a result, the network is intermittently disconnected.

Solution
--------

Add **connector.properties.retries=5** to the SQL statement.

.. code-block::

   create table kafka_sink(
        car_type string
       , car_name string
       , primary key (union_id) not enforced
   ) with (
       "connector.type" = "upsert-kafka",
       "connector.version" = "0.11",
       "connector.properties.bootstrap.servers" = "xxxx:9092",
       "connector.topic" = "kafka_car_topic ",
       "connector.sink.ignore-retraction" = "true",
       "connector.properties.retries" = "5",
       "format.type" = "json"
   );
