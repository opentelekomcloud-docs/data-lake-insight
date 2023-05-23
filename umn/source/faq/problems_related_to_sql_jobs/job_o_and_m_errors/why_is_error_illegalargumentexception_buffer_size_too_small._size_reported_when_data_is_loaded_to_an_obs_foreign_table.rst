:original_name: dli_03_0169.html

.. _dli_03_0169:

Why Is Error "IllegalArgumentException: Buffer size too small. size" Reported When Data Is Loaded to an OBS Foreign Table?
==========================================================================================================================

Symptom
-------

The following error message is displayed when the LOAD DATA command is executed by a Spark SQL job to import data to a DLI table:

.. code-block::

   error.DLI.0001: IllegalArgumentException: Buffer size too small. size = 262144 needed = 2272881

In some cases ,the following error message is displayed:

.. code-block::

   error.DLI.0999: InvalidProtocolBufferException: EOF in compressed stream footer position: 3 length: 479 range: 0 offset: 3 limit: 479 range 0 = 0 to 479 while trying to read 143805 bytes

Possible Causes
---------------

The data volume of the file to be imported is large and the value of **spark.sql.shuffle.partitions** is too large. As a result, the cache size is insufficient.

Solution
--------

Decrease the **spark.sql.shuffle.partitions** value. To set this parameter, perform the following steps:

#. Log in to the DLI management console and choose **Job Management** > **SQL Jobs**. In the **Operation** column of the target SQL job, click **Edit** to go to the **SQL Editor** page.
#. On the displayed page, click **Set Property** and set the parameter.
#. Execute the job again.
