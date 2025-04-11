:original_name: dli_03_0232.html

.. _dli_03_0232:

Data Writing Fails After a Flink SQL Job Consumed Kafka and Sank Data to the Elasticsearch Cluster
==================================================================================================

Symptom
-------

After a Flink SQL job consumed Kafka and sent data to the Elasticsearch cluster, the job was successfully executed, but no data is available.

Cause Analysis
--------------

Possible causes are as follows:

-  The data format is incorrect.
-  The data cannot be processed.

Procedure
---------

#. Check the task log on the Flink UI. The JSON body is contained in the error message, indicating that the data format is incorrect.
#. Check the data format. The Kafka data contains nested JSON bodies, which cannot be parsed.
#. Use either of the following methods to solve the problem:

   -  Create a JAR file with the UDF.
   -  Modify the configuration data.

#. Change the data format and execute the job again.
