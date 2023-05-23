:original_name: dli_03_0251.html

.. _dli_03_0251:

Error Message "org.postgresql.util.PSQLException: ERROR: tuple concurrently updated" Is Displayed When the System Executes insert overwrite on a Datasource GaussDB(DWS) Table
==============================================================================================================================================================================

Symptom
-------

The system failed to execute **insert overwrite** on the datasource GaussDB(DWS) table, and **org.postgresql.util.PSQLException: ERROR: tuple concurrently updated** was displayed.

Cause Analysis
--------------

Concurrent operations existed in the job. Two insert overwrite operations were executed on the table at the same time.

One CN was running the following statement:

.. code-block::

   TRUNCATE TABLE BI_MONITOR.SAA_OUTBOUND_ORDER_CUST_SUM

Another CN was running the following command:

.. code-block::

   call bi_monitor.pkg_saa_out_bound_monitor_p_saa_outbound_order_cust_sum

This function deletes and inserts SAA_OUTBOUND_ORDER_CUST_SUM.

Procedure
---------

Modify job logic to prevent concurrent insert overwrite operations on the same table.
