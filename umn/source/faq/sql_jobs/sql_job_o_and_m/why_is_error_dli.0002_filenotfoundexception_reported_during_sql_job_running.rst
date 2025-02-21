:original_name: dli_03_0189.html

.. _dli_03_0189:

Why Is Error "DLI.0002 FileNotFoundException" Reported During SQL Job Running?
==============================================================================

Symptom
-------

An error is reported during SQL job execution:

.. code-block::

   Please contact DLI service. DLI.0002: FileNotFoundException: getFileStatus on obs://xxx: status [404]

Solution
--------

Check whether there is another job that has deleted table information.

DLI does not allow multiple jobs to read and write the same table at the same time. Otherwise, job conflicts may occur and the jobs fail.
