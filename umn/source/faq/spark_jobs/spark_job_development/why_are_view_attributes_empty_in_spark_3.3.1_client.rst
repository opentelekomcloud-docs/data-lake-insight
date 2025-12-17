:original_name: dli_03_0279.html

.. _dli_03_0279:

Why Are View Attributes Empty in Spark 3.3.1 Client?
====================================================

Symptom
-------

When running a job with Spark 3.3.1, certain fields have empty default attribute values when queried in the view attribute on the client. In contrast, when using Spark 3.1.1, these fields have null default attribute values.

Root Cause Analysis
-------------------

Starting from Spark 3.3.0, the open-source community has upgraded the CreateViewStatement syntax and adjusted the syntax calling interface version to prevent null pointer exceptions when querying view attributes. However, this change caused the **DESC TABLE** command to display an empty string for the comment attribute if not configured.

This issue affects all operations involving default view attribute configurations in the Spark 3.3.1 client.

Solution
--------

If using the REST interface to call view queries, adjust the service logic to handle empty string results when executing jobs with Spark 3.3.1.
