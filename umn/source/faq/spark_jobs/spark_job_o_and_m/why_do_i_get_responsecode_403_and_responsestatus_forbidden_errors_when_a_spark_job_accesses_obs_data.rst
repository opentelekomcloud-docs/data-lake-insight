:original_name: dli_03_0156.html

.. _dli_03_0156:

Why Do I Get "ResponseCode: 403" and "ResponseStatus: Forbidden" Errors When a Spark Job Accesses OBS Data?
===========================================================================================================

Symptom
-------

The following error is reported when a Spark job accesses OBS data:

.. code-block::

   Caused by: com.obs.services.exception.ObsException: Error message:Request Error.OBS servcie Error Message. -- ResponseCode: 403, ResponseStatus: Forbidden

Solution
--------

Set the AK/SK to enable Spark jobs to access OBS data.

For details, see :ref:`How Do I Set Up AK/SK So That a General Queue Can Access Tables Stored in OBS? <dli_03_0017>`
