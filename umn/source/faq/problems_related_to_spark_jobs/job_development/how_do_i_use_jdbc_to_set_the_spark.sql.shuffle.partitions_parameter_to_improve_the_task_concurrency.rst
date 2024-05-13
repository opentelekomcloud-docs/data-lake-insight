:original_name: dli_03_0068.html

.. _dli_03_0068:

How Do I Use JDBC to Set the spark.sql.shuffle.partitions Parameter to Improve the Task Concurrency?
====================================================================================================

Scenario
--------

When shuffle statements, such as GROUP BY and JOIN, are executed in Spark jobs, data skew occurs, which slows down the job execution.

To solve this problem, you can configure **spark.sql.shuffle.partitions** to improve the concurrency of shuffle read tasks.

Configuring spark.sql.shuffle.partitions
----------------------------------------

You can use the **set** clause to configure the **dli.sql.shuffle.partitions** parameter in JDBC. The statement is as follows:

.. code-block::

   Statement st = conn.stamte()
   st.execute("set spark.sql.shuffle.partitions=20")
