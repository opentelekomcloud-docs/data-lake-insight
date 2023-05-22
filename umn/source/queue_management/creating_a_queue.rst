:original_name: dli_01_0363.html

.. _dli_01_0363:

Creating a Queue
================

Before executing a job, you need to create a queue.

.. note::

   -  If you use a sub-account to create a queue for the first time, log in to the DLI management console using the main account and keep records in the DLI database before creating a queue.
   -  It takes 6 to 10 minutes for a job running on a new queue for the first time.
   -  After a queue is created, if no job is run within one hour, the system releases the queue.

Procedure
---------

#. You can create a queue on the **Overview**, **SQL Editor**, or **Queue Management** page.

   -  In the upper right corner of the **Overview** page, click Create Queue.
   -  To create a queue on the **Queue Management** page:

      a. In the navigation pane of the DLI management console, choose **Resources** >\ **Queue Management**.
      b. In the upper right corner of the **Queue Management** page, click **Create Queue** to create a queue.

   -  To create a queue on the **SQL Editor** page:

      a. In the navigation pane of the DLI management console, click **SQL Editor**.
      b. On the left pane of the displayed **SQL Editor** page, click |image1| to the right of **Queues**.

#. In the displayed Create Queue dialog box, set related parameters by referring to :ref:`Table 1 <dli_01_0363__table17301125219910>`.

   .. _dli_01_0363__table17301125219910:

   .. table:: **Table 1** Parameters

      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                          |
      +===================================+======================================================================================================================================================================================================================================================+
      | Name                              | Name of a queue.                                                                                                                                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   | -  The queue name can contain only digits, letters, and underscores (_), but cannot contain only digits, start with an underscore (_), or be left unspecified.                                                                                       |
      |                                   | -  The length of the name cannot exceed 128 characters.                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   | .. note::                                                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   |    The queue name is case-insensitive. Uppercase letters will be automatically converted to lowercase letters.                                                                                                                                       |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Type                              | -  **For SQL**: compute resources used for SQL jobs.                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   | -  **For general purpose**: compute resources used for Spark and Flink jobs.                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   | -                                                                                                                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   |    .. note::                                                                                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   |       In **Dedicated Resource Mode**, you can create enhanced datasource connections.                                                                                                                                                                |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Specifications                    | Select queue specifications as required. A CU includes one core and 4 GB memory. You can set the total number of CUs on all compute nodes of a queue. DLI automatically allocates the memory and vCPUs for each node.                                |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   | -  Fixed specifications include **16 CUs**, **64 CUs**, **256 CUs**, and **512 CUs**.                                                                                                                                                                |
      |                                   | -  **Custom**: Set the specifications as required.                                                                                                                                                                                                   |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Description                       | Description of the queue to be created. The length of the queue name cannot exceed 256 characters.                                                                                                                                                   |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Advanced Settings                 | In the **Queue Type** area, select **Dedicated Resource Mode** and then click **Advanced Settings**.                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   | -  **Default**: The system automatically configures the parameter.                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   | -  **Custom**                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   |    **CIDR Block**: You can specify the CIDR block. For details, see :ref:`Modifying the Queue CIDR Block <dli_01_0443>`. If DLI enhanced datasource connection is used, the CIDR block of the DLI queue cannot overlap with that of the data source. |
      |                                   |                                                                                                                                                                                                                                                      |
      |                                   |    **Queue Type**: When running an AI-related SQL job, select **AI-enhanced**. When running other jobs, select **Basic**.                                                                                                                            |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **Create Now** to create a queue.

   After a queue is created, you can view and select the queue for use on the **Queue Management** page.

   .. note::

      It takes 6 to 10 minutes for a job running on a new queue for the first time.

.. |image1| image:: /_static/images/en-us_image_0237406526.png
