:original_name: dli_03_0172.html

.. _dli_03_0172:

Why Is Error "UQUERY_CONNECTOR_0001:Invoke DLI service api failed" Reported in the Job Log When I Use CDM to Migrate Data to DLI?
=================================================================================================================================

Symptom
-------

After the migration job is submitted, the following error information is displayed in the log:

.. code-block::

   org.apache.sqoop.common.SqoopException: UQUERY_CONNECTOR_0001:Invoke DLI service api failed, failed reason is %s.
   at org.apache.sqoop.connector.uquery.intf.impl.UQueryWriter.close(UQueryWriter.java:42)
   at org.apache.sqoop.connector.uquery.processor.Dataconsumer.run(Dataconsumer.java:217)
   at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
   at java.util.concurrent.FutureTask.run(FutureTask.java:266)
   at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
   at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
   at java.lang.Thread.run(Thread.java:748)

Possible Causes
---------------

When you create a migration job to DLI on the CDM console, you set **Resource Queue** to a DLI queue for general purpose. It should be a queue for SQL.

Solution
--------

#. On the DLI management console and click **Queue Management** in the navigation pane on the left. On the **Queue Management** page, check whether there are SQL queues.

   -  If there are, go to :ref:`3 <dli_03_0172__en-us_topic_0000001204269300_li15230171718316>`.
   -  If there are no SQL queues, go to :ref:`2 <dli_03_0172__en-us_topic_0000001204269300_li147241748216>` to buy an SQL queue.

#. .. _dli_03_0172__en-us_topic_0000001204269300_li147241748216:

   Click **Buy Queue** to create a queue. Set **Type** to **For SQL**, set other parameters required, and click **Buy**.

#. .. _dli_03_0172__en-us_topic_0000001204269300_li15230171718316:

   Go back to the CDM console and create a data migration job. Set **Resource Queue** to the created DLI SQL queue.

#. Submit the migration job and view the job execution logs.
