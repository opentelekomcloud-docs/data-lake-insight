:original_name: dli_03_0182.html

.. _dli_03_0182:

Why Does a SQL Job That Has Join Operations Stay in the Running State?
======================================================================

Symptom
-------

A SQL job contains join operations. After the job is submitted, it is stuck in the Running state and no result is returned.

Possible Causes
---------------

When a Spark SQL job has join operations on small tables, all executors are automatically broadcast to quickly complete the operations. However, this increases the memory consumption of the executors. If the executor memory usage is too high, the job fails to be executed.

Solution
--------

#. Check whether the **/*+ BROADCAST(u) \*/** falg is used to forcibly perform broadcast join in the executed SQL statement. If the flag is used, remove it.
#. Set **spark.sql.autoBroadcastJoinThreshold** to **-1**.

   a. Log in to the DLI management console and choose **Job Management** > **SQL Jobs**. In the **Operation** column of the failed job, click **Edit** to switch to the SQL editor page.
   b. Click **Settings** in the upper right corner. In the **Parameter Settings** area, add **spark.sql.autoBroadcastJoinThreshold** and set it to **-1**.
   c. Click **Execute** again to and view the job running result.
