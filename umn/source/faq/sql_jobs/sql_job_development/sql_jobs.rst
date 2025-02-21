:original_name: dli_03_0200.html

.. _dli_03_0200:

SQL Jobs
========

Can I Create Temporary Tables on DLI?
-------------------------------------

A temporary table is used to store intermediate results. When a transaction or session ends, the data in the temporary table can be automatically deleted. For example, in MySQL, you can use **create temporary table...** to create a temporary table. After a transaction or session ends, the table data is automatically deleted. Does DLI Support This Function?

Currently, **you cannot create temporary tables on DLI**. You can create a table by using SQL statements.

Can I Connect to DLI Locally? Is a Remote Connection Tool Supported?
--------------------------------------------------------------------

Currently DLI can only be accessed through a browser. You must submit jobs on the console.

Will a DLI SQL Job Be Killed If It Has Been Running for More Than 12 Hours?
---------------------------------------------------------------------------

By default, SQL jobs that have been running for more than 12 hours will be canceled to ensure stability of queues.

You can use the **dli.sql.job.timeout** parameter (unit: second) to configure the timeout interval.

Does DLI Support Local Testing of Spark Jobs?
---------------------------------------------

Currently, DLI does not support local testing of Spark jobs. You can install the DLI Livy tool and use its interactive sessions to debug Spark jobs.

Can I Delete a Row of Data from an OBS Table or DLI Table?
----------------------------------------------------------

Deleting a row of data from an OBS table or DLI table is not allowed.
