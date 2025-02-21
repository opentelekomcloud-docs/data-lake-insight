:original_name: dli_03_0265.html

.. _dli_03_0265:

Why Is Error "Timeout expired while fetching topic metadata" Repeatedly Reported in Flink JobManager Logs?
==========================================================================================================

If the Flink JobManager prompts "Timeout expired while fetching topic metadata", it means that the Flink job timed out while trying to fetch metadata for the Kafka topic.

In this case, you need to first check the network connectivity between the Flink job and Kafka, and ensure that the queue where the Flink job is executed can access the VPC network where Kafka is located.

If the network is unreachable, configure the network connectivity first and then re-execute the job.
