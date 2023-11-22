:original_name: dli_03_0161.html

.. _dli_03_0161:

Why Does a Flink Jar Job Fail to Access GaussDB(DWS) and a Message Is Displayed Indicating Too Many Client Connections?
=======================================================================================================================

Symptom
-------

When a Flink Jar job is submitted to access GaussDB(DWS), an error message is displayed indicating that the job fails to be started. The job log contains the following error information:

.. code-block::

   FATAL:  Already too many clients, active/non-active/reserved: 5/508/3

Possible Causes
---------------

The number of GaussDB(DWS) database connections exceeds the upper limit. In the error information, the value of **non-active** indicates the number of idle connections. For example, if the value of **non-active** is 508, there are 508 idle connections.

Solution
--------

Perform the following steps to solve the problem:

#. Log in to the GaussDB(DWS) command window and run the following SQL statement to release all idle (non-active) connections temporarily:

   .. code-block::

      SELECT PG_TERMINATE_BACKEND(pid) from pg_stat_activity WHERE state='idle';

#. Check whether the application actively releases the connections. If the application does not, optimize the code to release the connections.

#. On the GaussDB (DWS) management console, configure parameter **session_timeout**, which controls the timeout period of idle sessions. After an idle session's timeout period exceeds the specified value, the server automatically closes the connection.

   The default value of this parameter is **600** seconds. The value **0** indicates that the timeout limit is disabled. Do not set **session_timeout** to **0**.

   The procedure for setting parameter **session_timeout** is as follows:

   a. Log in to the GaussDB(DWS) management console.
   b. In the navigation pane on the left, click **Clusters**.
   c. In the cluster list, find the target cluster and click its name. The **Basic Information** page is displayed.
   d. Click the **Parameter Modifications** tab and modify the value of parameter **session_timeout**. Then click **Save**.
   e. In the **Modification Preview** dialog box, confirm the modification and click **Save**.
