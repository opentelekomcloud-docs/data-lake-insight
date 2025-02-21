:original_name: dli_03_0013.html

.. _dli_03_0013:

How Do I Do If the Compression Rate of an OBS Table Is High?
============================================================

When submitting a job to import data into a DLI table, if the compression rate of the Parquet/ORC file corresponding to the OBS table is high, exceeding 5 times the compression rate, you can optimize the job performance by adjusting the configuration.

Specifically, configure **dli.sql.files.maxPartitionBytes=33554432** in the **conf** field of the submit-job request body.

The default value of this configuration item is 128 MB. Configuring it to 32 MB can reduce the amount of data read by a single task and avoid processing a large amount of data by a single task after decompression due to a high compression rate.

However, adjusting this parameter may affect the execution efficiency and resource consumption of the job. Therefore, when making adjustments, you need to choose a suitable parameter value based on the actual amount of data and compression rate.
