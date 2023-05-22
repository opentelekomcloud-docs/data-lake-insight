:original_name: dli_01_0402.html

.. _dli_01_0402:

Overview
========

Queue
-----

Queues in DLI are computing resources, which are the basis for using DLI. All executed jobs require computing resources.

Currently, DLI provides two types of queues, **For SQL** and **For general use**. SQL queues are used to run SQL jobs. General-use queues are compatible with Spark queues of earlier versions and are used to run Spark and Flink jobs.

.. note::

   -  The SQL queue automatically restarts every 12 hours.
   -  Queues of different types cannot be directly switched. If you need to change a queue, purchase a new queue.
   -  The default queue is preset. Do not delete it.

Difference Between Computing and Storage Resources
--------------------------------------------------

.. table:: **Table 1** Difference between computing and storage resources

   +------------------+---------------------------------------------+-------------------------------------------------------+
   | Resource         | How to Obtain                               | Function                                              |
   +==================+=============================================+=======================================================+
   | Compute resource | Create queue on the DLI management console. | Used for executing queries.                           |
   +------------------+---------------------------------------------+-------------------------------------------------------+
   | Storage resource | DLI has a 5 GB quota.                       | Used for storing data in the database and DLI tables. |
   +------------------+---------------------------------------------+-------------------------------------------------------+

.. note::

   -  Storage resources are internal storage resources of DLI for storing database and DLI tables and represent the amount of data stored in DLI.
   -  By default, DLI provides a 5 GB quota for storage resources.
   -  A queue named **default** is preset in DLI. If you are uncertain about the required queue capacity or have no available queue capacity to run queries, you can execute jobs using this queue.

   -  The **default** queue is used only for user experience. It may be occupied by multiple users at a time. Therefore, it is possible that you fail to obtain the resource for related operations. You are advised to use a self-built queue to execute jobs.

Dedicated Queue
---------------

Resources of a dedicated queue are not released when the queue is idle. That is, resources are reserved regardless of whether the queue is used. Dedicated queues ensure that resources exist when jobs are submitted.

Elastic Scaling
---------------

DLI allows you to flexibly scale in or out queues on demand. After a queue with specified specifications is created, you can scale it in and out as required.

To change the queue specifications, see :ref:`Elastic Scaling <dli_01_0487>`.

.. note::

   Scaling can be performed for a newly created queue only when jobs are running on this queue.

Scheduled Elastic Scaling
-------------------------

DLI allows you to schedule tasks for periodic queue scaling. After creating a queue, the scheduled scaling tasks can be executed.

You can schedule an automatic scale-out/scale-in based on service requirements. The system periodically triggers queue scaling. For details, see :ref:`Scheduling CU Changes <dli_01_0488>`.

.. note::

   Scaling can be performed for a newly created queue only when jobs are running on this queue.

Automatic Queue Scaling
-----------------------

Flink jobs use queues. DLI can automatically trigger scaling for jobs based on the job size.

.. note::

   Scaling can be performed for a newly created queue only when there are jobs running on this queue.

Queue Management Page
---------------------

Queue Management provides the following functions:

-  :ref:`Managing Permissions <dli_01_0015>`
-  :ref:`Creating a Queue <dli_01_0363>`
-  :ref:`Deleting a Queue <dli_01_0016>`
-  :ref:`Modifying CIDR Block <dli_01_0443>`
-  :ref:`Elastic Scaling <dli_01_0487>`
-  :ref:`Scheduling CU Changes <dli_01_0488>`
-  :ref:`Testing Address Connectivity <dli_01_0489>`
-  :ref:`Creating a Topic for Key Event Notifications <dli_01_0421>`

.. note::

   To receive notifications when a DLI job fails, **SMN Administrator** permissions are required.

The queue list displays all queues created by you and the **default** queue. Queues are listed in chronological order by default in the queue list, with the most recently created queues displayed at the top.

.. table:: **Table 2** Parameter description

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                |
   +===================================+============================================================================================================================================================================================================================================================================================+
   | Name                              | Name of a queue.                                                                                                                                                                                                                                                                           |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Type                              | Queue type.                                                                                                                                                                                                                                                                                |
   |                                   |                                                                                                                                                                                                                                                                                            |
   |                                   | -  For SQL                                                                                                                                                                                                                                                                                 |
   |                                   | -  For general purpose                                                                                                                                                                                                                                                                     |
   |                                   | -  Spark queue (compatible with earlier versions)                                                                                                                                                                                                                                          |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Specifications                    | Queue size. Unit: CU                                                                                                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                            |
   |                                   | CU is the pricing unit of queues. A CU consists of 1 vCPU and 4-GB memory. The computing capabilities of queues vary with queue specifications. The higher the specifications, the stronger the computing capability.                                                                      |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Actual CUs                        | Actual size of the current queue.                                                                                                                                                                                                                                                          |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Elastic Scaling                   | Target CU value for scheduled scaling, or the maximum and minimum CU values of the current specifications.                                                                                                                                                                                 |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Username                          | Queue owner                                                                                                                                                                                                                                                                                |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Description                       | Description of a queue specified during queue creation. If no description is provided, **--** is displayed.                                                                                                                                                                                |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operation                         | -  **Delete**: Allow you to delete the selected queue. You cannot delete a queue where there are running jobs or jobs are being submitted.                                                                                                                                                 |
   |                                   | -  **Manage Permissions**: You can view the user permissions corresponding to the queue and grant permissions to other users.                                                                                                                                                              |
   |                                   | -  **More**                                                                                                                                                                                                                                                                                |
   |                                   |                                                                                                                                                                                                                                                                                            |
   |                                   |    -  **Restart**: Forcibly restart a queue.                                                                                                                                                                                                                                               |
   |                                   |                                                                                                                                                                                                                                                                                            |
   |                                   |       .. note::                                                                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                            |
   |                                   |          Only the SQL queue has the **Restart** operation.                                                                                                                                                                                                                                 |
   |                                   |                                                                                                                                                                                                                                                                                            |
   |                                   |    -  **Elastic Scaling**: You can select **Scale-out** or **Scale-in** as required. The number of CUs after modification must be an integer multiple of 16.                                                                                                                               |
   |                                   |    -  **Schedule CU Changes**: You can set different queue sizes at different time or in different periods based on the service period or usage. The system automatically performs scale-out or scale-in as scheduled. The **After Modification** value must be an integer multiple of 16. |
   |                                   |    -  **Modifying CIDR Block**: When DLI enhanced datasource connection is used, the CIDR block of the DLI queue cannot overlap with that of the data source. You can modify the CIDR block as required.                                                                                   |
   |                                   |    -  **Test Address Connectivity**: Test whether the queue is reachable to the specified address. The domain name and IP address are supported. The port can be specified.                                                                                                                |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
