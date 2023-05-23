:original_name: dli_03_0178.html

.. _dli_03_0178:

What Can I Do If an Error Is Reported When the Execution of the API for Creating a SQL Job Times Out?
=====================================================================================================

Symptom
-------

When the API call for submitting a SQL job times out, and the following error information is displayed:

.. code-block::

   There are currently no resources tracked in the state, so there is nothing to refresh.

Possible Causes
---------------

The timeout of API calls in synchronous is two minutes. If a call times out, an error will be reported.

Solution
--------

When you make a call of the API for submitting a SQL job, set **dli.sql.sqlasync.enabled** to **true** to run the job asynchronously.
