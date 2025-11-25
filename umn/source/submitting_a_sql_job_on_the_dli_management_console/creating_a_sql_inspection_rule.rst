:original_name: dli_01_0612.html

.. _dli_01_0612:

Creating a SQL Inspection Rule
==============================

What Is SQL Inspection?
-----------------------

There are numerous SQL engines in the big data field, which bring diversity to the solutions but also expose some issues such as varying quality of SQL input statements, difficult SQL problem localization, and excessive resource consumption by large SQL statements.

Poor quality SQL can have unforeseeable impacts on data analysis platforms, affecting system performance or platform stability.

DLI offers this feature to allow you to create inspection rules for the Spark SQL engine. This helps prevent common issues like large or low-quality SQL statements by providing pre-event information, blocking, and in-event circuit breaker. You do not need to change your SQL submission method or syntax, so it is easy to implement without affecting service operations.

-  You can configure SQL inspection rules in a visualized manner and also have the ability to query and modify these rules.
-  During query execution and response, each SQL engine proactively inspects SQL statements based on the rules.
-  Administrators can choose to display hints on, intercept, or block SQL statements. The system logs SQL inspection events in real time for SQL audit. O&M engineers can analyze the logs, assess the quality of SQL statements on the live network, identify potential risks, and take preventive measures.

This section describes how to create a SQL inspection rule to enhance SQL defense capabilities.

Notes and Constraints of DLI SQL Inspection Rules
-------------------------------------------------

-  SQL inspection is only supported by Spark 3.3.\ *x* or later.

-  Only one inspection rule can be created for an action or a queue.

-  Each rule can be associated with a maximum of 50 SQL queues.

-  A maximum of 1,000 rules can be created for each project.

-  Do not delete the preset system inspection rules of DLI.

-  You are advised not to delete the default rules created by DLI to prevent queue resources from being used up by large SQL jobs.

   Default system rules include **Scan files number**, **Scan partitions number**, **Shuffle data(GB)**, **Count(distinct) occurrences**, and **Not in<Subquery>**. For details, see :ref:`Table 2 <dli_01_0612__table1626674755019>`.


Creating a SQL Inspection Rule
------------------------------

You can create SQL inspection rules for specified SQL queues on the **SQL Inspector** page. The system will prompt, block, or perform circuit breaking on SQL requests that trigger the rules.

.. note::

   When creating or modifying a SQL inspection rule, evaluate the appropriateness of enabling the rules and setting the threshold based on the service scenario. This will help avoid the negative impact of unreasonable inspection rules blocking or performing circuit breaking on relevant SQL requests on the services.

#. Log in to the DLI management console.

#. In the navigation pane on the left, choose **Global Configuration** > **SQL Inspector**.

#. On the displayed **SQL Inspector** page, click **Create Rule** in the upper right corner. In the **Create Rule** dialog box, set parameters based on the table below.

   .. table:: **Table 1** Parameters for creating a SQL inspection rule

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                             |
      +===================================+=========================================================================================================================================================================================+
      | Rule Name                         | Name of a SQL inspection rule                                                                                                                                                           |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | System Rules                      | Select an inspection rule. For details about the system inspection rules supported by DLI, see :ref:`SQL Inspection System Rules That DLI Supports <dli_01_0612__section516925416473>`. |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Queues                            | Select the queues the rules are bound to.                                                                                                                                               |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Description                       | Enter a rule description.                                                                                                                                                               |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Rule Action                       | Actions that the current SQL inspection rule supports.                                                                                                                                  |
      |                                   |                                                                                                                                                                                         |
      |                                   | SQL rules support the following types of actions:                                                                                                                                       |
      |                                   |                                                                                                                                                                                         |
      |                                   | -  **Info**: Record logs and provide a hint for handling the SQL request. If the rule has parameters, you need to configure the threshold.                                              |
      |                                   | -  **Block**: Intercept the SQL request that meets the rule. If the rule has parameters, you need to configure the threshold.                                                           |
      |                                   | -  **Circuit Breaker**: Perform circuit breaking on the SQL request that meets the inspection rules. If the rule has parameters, you need to configure the threshold.                   |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

   View the added inspection rule on the **SQL Inspector** page. The rule takes effect dynamically.

   To modify a rule, click **Modify** in its **Operation** column.

.. _dli_01_0612__section516925416473:

SQL Inspection System Rules That DLI Supports
---------------------------------------------

This part describes the system inspection rules supported by DLI. For details, see :ref:`Table 2 <dli_01_0612__table1626674755019>`.

-  Default system rules are SQL inspection rules automatically created by DLI when a queue is created. These rules are bound to the queue and cannot be deleted.

-  Default system rules include **Scan files number**, **Scan partitions number**, **Shuffle data(GB)**, **Count(distinct) occurrences**, and **Not in<Subquery>**.

-  Only one inspection rule can be created for an action or a queue.

-  For every supported action, a default system rule is created. For example, when a queue is created, a **Scan files number** rule is automatically created for both the **Info** and **Block** actions.

-  Different engine versions support different inspection rules.

   To view the engine version of a queue, choose **Resources** > **Queue Management** in the navigation pane on the left, select the queue, double-click the pane at the bottom of the page, and check the value of **Default Version**.

.. _dli_01_0612__table1626674755019:

.. table:: **Table 2** System inspection rules supported by DLI

   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | Rule ID      | Rule Name                   | Description                                                                                                                  | Type    | Applicable Engine | Action          | Value                     | Default System Rule | Example SQL Statement                                                  | Supported Engine Version |
   +==============+=============================+==============================================================================================================================+=========+===================+=================+===========================+=====================+========================================================================+==========================+
   | dynamic_0001 | Scan files number           | Maximum number of files to be scanned                                                                                        | Dynamic | Spark             | Info            | Value range: 1-2000000    | Yes                 | N/A                                                                    | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         | HetuEngine        | Block           | Default value: **200000** |                     |                                                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | dynamic_0002 | Scan partitions number      | Maximum number of partitions involved in the operations (select, delete, update, and alter) that can be performed on a table | Dynamic | Spark             | Info            | Value range: 1-500000     | Yes                 | select \* from Partitioned table                                       | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   | Block           | Default value: **5000**   |                     |                                                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | running_0002 | Memory used(MB)             | Peak memory usage of the SQL statement                                                                                       | Running | Spark             | Circuit Breaker | Value range: 1-8388608    | No                  | N/A                                                                    | Spark 3.3.1              |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | running_0003 | Run time(S)                 | Maximum running duration of the SQL statement                                                                                | Running | Spark             | Circuit Breaker | Unit: second              | No                  | N/A                                                                    | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 | Value range: 1-43200      |                     |                                                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | running_0004 | Scan data(GB)               | Maximum amount of data to be scanned                                                                                         | Running | Spark             | Circuit Breaker | Unit: GB                  | No                  | N/A                                                                    | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 | Value range: 1-10240      |                     |                                                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | running_0005 | Shuffle data(GB)            | Maximum amount of data to be shuffled                                                                                        | Running | Spark             | Circuit Breaker | Unit: GB                  | Yes                 | N/A                                                                    | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 | Value range: 1-10240      |                     |                                                                        | Spark 2.4.5              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 | Default value: **2048**   |                     |                                                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | static_0001  | Count(distinct) occurrences | Maximum number of occurrences of count(distinct) in the SQL statement                                                        | Static  | Spark             | Info            | Value range: 1-100        | Yes                 | SELECT COUNT(DISTINCT deviceId), COUNT(DISTINCT collDeviceId)          | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   | Block           | Default value: **10**     |                     | FROM table                                                             |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     | GROUP BY deviceName, collDeviceName, collCurrentVersion;               |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | static_0002  | Not in<Subquery>            | Check whether **not in <subquery>** is used in the SQL statement.                                                            | Static  | Spark             | Info            | Value range: Yes, No      | Yes                 | SELECT \*                                                              | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   | Block           | Default value: **Yes**    |                     | FROM Orders o                                                          |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     | WHERE Orders.Order_ID not in (Select Order_ID                          |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     | FROM HeldOrders h                                                      |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     | where h.order_id = o.order_id);                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | static_0003  | Join occurrences            | Maximum number of joins in the SQL statement                                                                                 | Static  | Spark             | Info            | Value range: 1-50         | No                  | SELECT name, text FROM table_1 JOIN table_2 ON table_1.Id = table_2.Id | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   | Block           |                           |                     |                                                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | static_0004  | Union occurrences           | Maximum number of union all times in the SQL statement                                                                       | Static  | Spark             | Info            | Value range: 1-100        | No                  | select \* from tables t1                                               | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   | Block           |                           |                     | union all select \* from tables t2                                     |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     | union all select \* from tables t3                                     |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | static_0005  | Subquery nesting layers     | Maximum number of subquery nesting layers                                                                                    | Static  | Spark             | Info            | Value range: 1-20         | No                  | select \* from (                                                       | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   | Block           |                           |                     | with temp1 as (select \* from tables)                                  |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     | select \* from temp1);                                                 |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | static_0006  | Sql size(KB)                | Maximum size of a SQL file                                                                                                   | Static  | Spark             | Info            | Unit: KB                  | No                  | N/A                                                                    | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   | Block           | Value range: 1-1024       |                     |                                                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
   | static_0007  | Cartesian product           | Limitation of Cartesian products when multiple tables are being associated                                                   | Static  | Spark             | Info            | Value range: 0-1          | No                  | select \* from A,B;                                                    | Spark 3.3.1              |
   |              |                             |                                                                                                                              |         |                   |                 |                           |                     |                                                                        |                          |
   |              |                             |                                                                                                                              |         |                   | Block           |                           |                     |                                                                        |                          |
   +--------------+-----------------------------+------------------------------------------------------------------------------------------------------------------------------+---------+-------------------+-----------------+---------------------------+---------------------+------------------------------------------------------------------------+--------------------------+
