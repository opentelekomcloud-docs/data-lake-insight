:original_name: dli_03_0192.html

.. _dli_03_0192:

Why Does the Job Fail to Be Executed Due to Insufficient Database and Table Permissions?
========================================================================================

Symptom
-------

When a Spark job is running, an error message is displayed, indicating that the user does not have the database permission. The error information is as follows:

.. code-block::

   org.apache.spark.sql.AnalysisException: org.apache.hadoop.hive.ql.metadata.HiveException: MetaException(message:Permission denied for resource: databases.xxx,action:SPARK_APP_ACCESS_META)

Solution
--------

You need to assign the database permission to the user who executes the job. The procedure is as follows:

#. In the navigation pane on the left of the management console, choose **Data Management** > **Databases and Tables**.
#. Locate the row where the target database resides and click **Permissions** in the **Operation** column.
#. On the displayed page, click **Grant Permission** in the upper right corner.
#. In the displayed dialog box, select **User** or **Project**, enter the username or select the project that needs the permission, and select the desired permissions.
#. Click **OK**.
