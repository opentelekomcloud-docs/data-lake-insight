:original_name: dli_01_0638.html

.. _dli_01_0638:

Viewing a SQL Execution Plan
============================

A SQL execution plan is a logical flowchart of a database query that shows how a database management system executes a specific SQL query. The execution plan details the steps needed to execute the query, such as table scans, index lookups, join operations (for example, inner join, outer join), sorting, and aggregation. Viewing an execution plan can help analyze query performance, identify potential performance bottlenecks, understand the query's execution logic, and use this information to adjust the query or database structure to improve SQL query efficiency.

This section describes how to view a SQL execution plan on the DLI management console.

Notes and Constraints
---------------------

-  You can only view SQL execution plans for Spark 3.3.\ *x* or later queues and HetuEngine queues.
-  You can only view a SQL execution plan after a SQL job is executed.
-  You can only view the SQL execution plan for SQL jobs that have reached the **Finished** state.
-  Make sure you have authorized DLI to use OBS buckets for saving the SQL execution plans of user jobs.
-  SQL execution plans are stored in paid storage buckets for DLI jobs. The system does not automatically delete them. You are advised to configure the bucket lifecycle and specify rules to regularly delete or migrate unused SQL execution plans. Refer to :ref:`Configuring a DLI Job Bucket <dli_01_0536>`.

Procedure
---------

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Job Management** > **SQL Jobs**.

#. Select the SQL job you want to query.

#. At the bottom of the page, click the name of the job you selected to view its details.

   In the details area, click **Expand** next to **SQL Execution Plan**. The system queries the SQL execution plan of the job from the DLI job bucket and displays the plan on the console.

   If the SQL execution plan in the DLI job bucket is deleted, the plan may not be displayed because the source file is missing.
