:original_name: dli_08_15060.html

.. _dli_08_15060:

Print
=====

Function
--------

The Print connector is used to print output data to the error file or out file in the TaskManager, making it easier for you to view the result in code debugging.

Prerequisites
-------------

None

Caveats
-------

-  The Print result table supports the following output formats:

   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
   | Print                           | Condition 1                                                                                                                                                | Condition 2      |
   +=================================+============================================================================================================================================================+==================+
   | Identifier:Task ID> Output data | A print identifier prefix must be provided. That is, you must specify **print-identifier** in the **WITH** parameter when creating the Print result table. | parallelism > 1  |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
   | Identifier> Output data         | A print identifier prefix must be provided. That is, you must specify **print-identifier** in the **WITH** parameter when creating the Print result table. | parallelism == 1 |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
   | Task ID> Output data            | A print identifier prefix is not needed. That is, you do not specify **print-identifier** in the **WITH** parameter when creating the Print result table.  | parallelism > 1  |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
   | Output data                     | A print identifier prefix is not needed. That is, you do not specify **print-identifier** in the **WITH** parameter when creating the Print result table.  | parallelism == 1 |
   +---------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

Syntax
------

::

   create table printSink (
     attr_name attr_type
     (',' attr_name attr_type) *
     (',' PRIMARY KEY (attr_name,...) NOT ENFORCED)
   ) with (
     'connector' = 'print',
     'print-identifier' = '',
     'standard-error' = ''
   );

Parameter Description
---------------------

.. table:: **Table 1** Parameters

   +------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Mandatory   | Default Value | Data Type   | Description                                                                                                                                                                 |
   +==================+=============+===============+=============+=============================================================================================================================================================================+
   | connector        | Yes         | None          | String      | Connector to be used. Set this parameter to **print**.                                                                                                                      |
   +------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | print-identifier | No          | None          | String      | Message that identifies print and is prefixed to the output of the value.                                                                                                   |
   +------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | standard-error   | No          | false         | Boolean     | The value can be only **true** or **false**. The default value is **false**.                                                                                                |
   |                  |             |               |             |                                                                                                                                                                             |
   |                  |             |               |             | -  If the value is **true**, data is output to the error file of the TaskManager.                                                                                           |
   |                  |             |               |             | -  If the value is **false**, data is output to the **out** file of the TaskManager.                                                                                        |
   +------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sink.parallelism | No          | None          | Integer     | Defines the parallelism of the Print result table. By default, the parallelism is determined by the framework: using the same parallelism as the upstream chained operator. |
   +------------------+-------------+---------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Create a Flink OpenSource SQL job. Run the following script to generate random data through the DataGen table and output the data to the Print result table.

.. code-block::

   create table dataGenSource(
     user_id string,
     amount int
   ) with (
     'connector' = 'datagen',
     'rows-per-second' = '1', --Generates a piece of data per second.
     'fields.user_id.kind' = 'random', --Specifies a random generator for the user_id field.
     'fields.user_id.length' = '3' --Limits the length of user_id to 3.
   );

   create table printSink(
     user_id string,
     amount int
   ) with (
     'connector' = 'print',
     'print-identifier' = '', --Configure the data prefix.
     'standard-error' = 'false', --Output data to the out file of TaskManager.
     'sink.parallelism' = '2' --Configure the parallelism.
   );

   insert into printSink select * from dataGenSource;

After the job is submitted, the job status changes to **Running**. You can perform the following operations of either method to view the output result:

-  Method 1:

   #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   #. Locate the row that contains the target Flink job, and choose **More** > **FlinkUI** in the **Operation** column.
   #. On the Flink UI, choose **Task Managers**, click the task name, and select **Stdout** to view job logs.

-  Method 2: If you select **Save Job Log** on the **Running Parameters** tab before submitting the job, perform the following operations:

   #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   #. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   #. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.

-  Method 3: If the queue is a new version, perform the following operations:

   #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   #. In the job list, click the name of your desired Flink job. On the displayed page, click the **Logs** tab.
   #. Select the corresponding TaskManager name from the drop-down list in the upper left corner and click the **taskmanager.out** file to view its result log.
