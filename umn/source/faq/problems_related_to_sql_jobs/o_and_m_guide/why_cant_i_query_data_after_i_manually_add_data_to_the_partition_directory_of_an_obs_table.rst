:original_name: dli_03_0190.html

.. _dli_03_0190:

Why Can't I Query Data After I Manually Add Data to the Partition Directory of an OBS Table?
============================================================================================

Symptom
-------

Partition data is manually uploaded to a partition of an OBS table. However, the data cannot be queried using DLI SQL editor.

Solution
--------

After manually adding partition data, you need to update the metadata information of the OBS table. Run the following statement on desired table:

.. code-block::

   MSCK REPAIR TABLE table_name;

Query the data in the OBS partitioned table.
