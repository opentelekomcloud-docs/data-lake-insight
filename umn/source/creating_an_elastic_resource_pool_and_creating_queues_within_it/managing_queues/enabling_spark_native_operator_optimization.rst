:original_name: dli_01_0685.html

.. _dli_01_0685:

Enabling Spark Native Operator Optimization
===========================================

Scenario
--------

Spark Native is a core component of Apache Spark designed to enhance the performance of Spark SQL computations. By utilizing vectorized C++ acceleration libraries, it accelerates the performance of Spark operators. Enabling Spark Native can improve the performance of Spark SQL jobs, reducing CPU and memory consumption.

After enabling Spark Native in a queue, it currently supports optimization for Scan and Filter operators.

-  **Scan**: The Scan operator is typically triggered by query statements, such as **select \* from test_table**.

   The following conditions support enabling Native:

   -  Hive tables and datasource tables in Parquet format
   -  Datasource tables in ORC format

-  **Filter**: The Filter operator is typically triggered by **WHERE** clauses, such as **select \* from test_table where id =** *xxx*.

.. note::

   Using the **EXPLAIN** statement, you can view the types of operators triggered by SQL commands, for example, **Explain select \* from test_table**.

This section describes how to enable Spark Native operator optimization.

Notes and Constraints
---------------------

-  To enable the Spark Native engine for a queue in an elastic resource pool, the following conditions must be met simultaneously:

   -  Type of an elastic resource pool: **Standard**
   -  Type of a queue: **For SQL**
   -  Spark version: Spark 3.3.1 or later

-  For the **default** queue, when Spark 3.3.1 or later is used, Spark Native is disabled by default.
-  To disable Spark Native for a job, configure **spark.gluten.enabled=false** in the job parameters to disable Spark Native at the job level.


Enabling Spark Native Operator Optimization
-------------------------------------------

-  **When creating a SQL queue in an elastic resource pool, you can enable Spark Native.**

   Enable **Spark Native** acceleration on DLI.

   For details, see :ref:`Creating an Elastic Resource Pool and Creating Queues Within It <dli_01_0505>`.

-  **For SQL queues in an existing elastic resource pool, you can enable Spark Native by setting queue properties.**

   #. In the navigation pane on the left of the DLI management console, choose **Resources** > **Queue Management**.

   #. Locate the queue for which you want to set properties, click **More** in the **Operation** column, and select **Set Property**.

   #. Go to the queue property setting page and set property parameters. :ref:`Table 1 <dli_01_0685__table206971632142710>` describes the property parameters.

      .. note::

         For created queues, if you change the Spark Native setting (enabled/disabled) through the DLI management console or API, you need to restart the queue for the modification to take effect.

      .. _dli_01_0685__table206971632142710:

      .. table:: **Table 1** Queue properties

         +-------------------------------+-----------------------------------------------------------------------------------------------------------+---------------+
         | Property                      | Description                                                                                               | Example Value |
         +===============================+===========================================================================================================+===============+
         | DLI Spark Native Acceleration | Enabling Spark Native can improve the performance of Spark SQL jobs, reducing CPU and memory consumption. | Enabled       |
         +-------------------------------+-----------------------------------------------------------------------------------------------------------+---------------+

   #. Click **OK**.

Disabling Spark Native Operator Optimization
--------------------------------------------

-  **Disable Spark Native for SQL queues in an elastic resource pool.**

   #. In the navigation pane on the left of the DLI management console, choose **Resources** > **Queue Management**.

   #. Locate the queue for which you want to set properties, click **More** in the **Operation** column, and select **Set Property**.

   #. Go to the queue property setting page and set property parameters. :ref:`Table 2 <dli_01_0685__table15120134595811>` describes the property parameters.

      .. _dli_01_0685__table15120134595811:

      .. table:: **Table 2** Queue properties

         +-------------------------------+-----------------------------------------------------------------------------------------------------------+---------------+
         | Property                      | Description                                                                                               | Example Value |
         +===============================+===========================================================================================================+===============+
         | DLI Spark Native Acceleration | Enabling Spark Native can improve the performance of Spark SQL jobs, reducing CPU and memory consumption. | Disabled      |
         +-------------------------------+-----------------------------------------------------------------------------------------------------------+---------------+

   #. Click **OK**.

-  **Disable Spark Native for a specified job when a queue has Spark Native enabled.**

   After Spark Native is enabled for a SQL queue, if you want to disable Spark Native for a particular job running in the queue,

   add **spark.gluten.enabled=false** to the parameter settings of the SQL job to disable Spark Native.
