:original_name: dli_08_0382.html

.. _dli_08_0382:

DataGen Source Table
====================

Function
--------

DataGen is used to generate random data for debugging and testing.

Prerequisites
-------------

None

Precautions
-----------

-  When you create a DataGen table, the table field type cannot be Array, Map, or Row. You can use **COMPUTED COLUMN** in :ref:`CREATE TABLE <dli_08_0375>` to construct similar functions.
-  When creating a Flink OpenSource SQL job, you need to set **Flink Version** to **1.12** on the **Running Parameters** tab of the job editing page, select **Save Job Log**, and set the OBS bucket for saving job logs.

Syntax
------

.. code-block::

   create table dataGenSource(
     attr_name attr_type
     (',' attr_name attr_type)*
     (',' WATERMARK FOR rowtime_column_name AS watermark-strategy_expression)
   )
   with (
     'connector' = 'datagen'
   );

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory   | Default Value                                      | Data Type                     | Description                                                                                                                                                                                                                                  |
   +=================+=============+====================================================+===============================+==============================================================================================================================================================================================================================================+
   | connector       | Yes         | None                                               | String                        | Connector to be used. Set this parameter to **datagen**.                                                                                                                                                                                     |
   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rows-per-second | No          | 10000                                              | Long                          | Number of rows generated per second, which is used to control the emit rate.                                                                                                                                                                 |
   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | fields.#.kind   | No          | random                                             | String                        | Generator of the **#** field. The **#** field must be an actual field in the DataGen table. Replace **#** with the corresponding field name. The meanings of the **#** field for other parameters are the same.                              |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               | The value can be **sequence** or **random**.                                                                                                                                                                                                 |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               | -  **random** is the default generator. You can use the **fields.#.max** and **fields.#.min** parameters to specify the maximum and minimum values that are randomly generated.                                                              |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               |    If the specified field type is char, varchar, or string, you can also use the **fields.#.length** field to specify the length. A random generator is an unbounded generator.                                                              |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               | -  Sequence generator. You can use **fields.#.start** and **fields.#.end** to specify the start and end values of a sequence. A sequence generator is a bounded generator. When the sequence number reaches the end value, the reading ends. |
   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | fields.#.min    | No          | Minimum value of the field type specified by **#** | Field type specified by **#** | This parameter is valid only when **fields.#.kind** is set to **random**.                                                                                                                                                                    |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               | Minimum value of the random generator. It applies only to numeric field types specified by **#**.                                                                                                                                            |
   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | fields.#.max    | No          | Maximum value of the field type specified by **#** | Field type specified by **#** | This parameter is valid only when **fields.#.kind** is set to **random**.                                                                                                                                                                    |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               | Maximum value of the random generator. It applies only to numeric field types specified by **#**.                                                                                                                                            |
   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | fields.#.length | No          | 100                                                | Integer                       | This parameter is valid only when **fields.#.kind** is set to **random**.                                                                                                                                                                    |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               | Length of the characters generated by the random generator. It applies only to char, varchar, and string types specified by **#**.                                                                                                           |
   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | fields.#.start  | No          | None                                               | Field type specified by **#** | This parameter is valid only when **fields.#.kind** is set to **sequence**.                                                                                                                                                                  |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               | Start value of a sequence generator.                                                                                                                                                                                                         |
   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | fields.#.end    | No          | None                                               | Field type specified by **#** | This parameter is valid only when **fields.#.kind** is set to **sequence**.                                                                                                                                                                  |
   |                 |             |                                                    |                               |                                                                                                                                                                                                                                              |
   |                 |             |                                                    |                               | End value of a sequence generator.                                                                                                                                                                                                           |
   +-----------------+-------------+----------------------------------------------------+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Create a Flink OpenSource SQL job. Run the following script to generate random data through the DataGen table and output the data to the Print result table.

When you create a job, set **Flink Version** to **1.12** on the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.

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
     'connector' = 'print'
   );

   insert into printSink select * from dataGenSource;

After the job is submitted, the job status changes to **Running**. You can perform the following operations to view the output result:

-  Method 1:

   #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   #. Locate the row that contains the target Flink job, and choose **More** > **FlinkUI** in the **Operation** column.
   #. On the Flink UI, choose **Task Managers**, click the task name, and select **Stdout** to view job logs.

-  Method 2: If you select **Save Job Log** on the **Running Parameters** tab before submitting the job, perform the following operations:

   #. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Flink Jobs**.
   #. Click the name of the corresponding Flink job, choose **Run Log**, click **OBS Bucket**, and locate the folder of the log you want to view according to the date.
   #. Go to the folder of the date, find the folder whose name contains **taskmanager**, download the **taskmanager.out** file, and view result logs.
