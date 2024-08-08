:original_name: dli_03_0196.html

.. _dli_03_0196:

How Do I Troubleshoot Slow SQL Jobs?
====================================

If the job runs slowly, perform the following steps to find the causes and rectify the fault:

Possible Cause 1: Full GC
-------------------------

**Check whether the problem is caused by FullGC.**

#. Log in to the DLI console. In the navigation pane, choose **Job Management** > **SQL Jobs**.
#. On the **SQL Jobs** page, locate the row that contains the target job and click **More** > **View Log** in the **Operation** column.
#. Obtain the folder of the archived logs in the OBS directory. The details are as follows:

   -  Spark SQL jobs:

      Locate the log folder whose name contains **driver** or **container\_** *xxx* **\_000001**.

   -  Spark Jar jobs:

      The archive log folder of a Spark Jar job starts with **batch**.

#. Go to the archive log file directory and download the **gc.log.\* log** file.
#. Open the downloaded **gc.log.\* log** file, search for keyword **Full GC**, and check whether time records in the file are continuous and Full GC information is recorded repeatedly.

**Cause locating and solution**

**Cause 1: There are too many small files in a table.**

#. Log in to the DLI console and go to the SQL editor page. On the SQL Editor page, select the queue and database of the faulty job.

#. Run the following statement to check the number of files in the table and specify the *table name*.

   .. code-block::

      select count(distinct fn)  FROM
      (select input_file_name() as fn from table name) a

#. If there are too many small files, rectify the fault by referring to :ref:`How Do I Merge Small Files? <dli_03_0086>`.

**Cause 2: There is a broadcast table.**

#. Log in to the DLI console. In the navigation pane, choose **Job Management** > **SQL Jobs**.

#. On the **SQL Jobs** page, locate the row that contains the target job and click |image1| to view the job details and obtain the job ID.

#. In the **Operation** column of the job, click **Spark UI**.

#. On the displayed page, choose **SQL** from the menu bar. Click the hyperlink in the **Description** column of the row that contains the job ID.

#. View the DAG of the job to check whether the BroadcastNestedLoopJoin node exists.


   .. figure:: /_static/images/en-us_image_0000001352514025.png
      :alt: **Figure 1** DAG

      **Figure 1** DAG

#. If the BroadcastNestedLoopJoin node exists, refer to :ref:`Why Does a SQL Job That Has Join Operations Stay in the Running State? <dli_03_0182>` to rectify the fault.

Possible Cause 2: Data Skew
---------------------------

**Check whether the problem is caused by data skew.**

#. Log in to the DLI console. In the navigation pane, choose **Job Management** > **SQL Jobs**.

#. On the **SQL Jobs** page, locate the row that contains the target job and click |image2| to view the job details and obtain the job ID.

#. In the **Operation** column of the job, click **Spark UI**.

#. On the displayed page, choose **SQL** from the menu bar. Click the hyperlink in the **Description** column of the row that contains the job ID.

   |image3|

#. View the running status of the current stage in the Active Stage table on the displayed page. Click the hyperlink in the **Description** column.

   |image4|

#. View the **Launch Time** and **Duration** of each task.

#. Click **Duration** to sort tasks. Check whether the overall job duration is prolonged because a task has taken a long time.

   According to :ref:`Figure 2 <dli_03_0196__en-us_topic_0000001299622958_fig108266043919>`, when data skew occurs, the data volume of shuffle reads of a task is much greater than that of other tasks.

   .. _dli_03_0196__en-us_topic_0000001299622958_fig108266043919:

   .. figure:: /_static/images/en-us_image_0000001299958066.png
      :alt: **Figure 2** Data skew

      **Figure 2** Data skew

**Cause locating and solution**

Shuffle data skew is caused by unbalanced number of key values in join.

#. Perform **group by** and **count** on a join to collect statistics on the number of key values of each join. The following is an example:

   Join table **lefttbl** and table **righttbl**. **num** in the **lefttbl** table is the key value of the join. You can perform **group by** and **count** on **lefttbl.num**.

   .. code-block::

      SELECT * FROM lefttbl a LEFT join righttbl b on a.num = b.int2;
      SELECT count(1) as count,num from lefttbl  group by lefttbl.num ORDER BY count desc;

#. Use **concat(cast(round(rand() \* 999999999) as string)** to generate a random number for each key value.

#. If the skew is serious and random numbers cannot be generated, see :ref:`How Do I Do When Data Skew Occurs During the Execution of a SQL Job? <dli_03_0093>`

.. |image1| image:: /_static/images/en-us_image_0000001299472334.png
.. |image2| image:: /_static/images/en-us_image_0000001299478654.png
.. |image3| image:: /_static/images/en-us_image_0000001299475238.png
.. |image4| image:: /_static/images/en-us_image_0000001299635390.png
