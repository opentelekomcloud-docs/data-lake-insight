:original_name: dli_03_0208.html

.. _dli_03_0208:

Why Is Error "DLI.0003: AccessControlException XXX" Reported When I Access a SQL Job?
=====================================================================================

Symptom
-------

Error message "DLI.0003: AccessControlException XXX" is reported when a SQL job is accessed.

Solution
--------

Check the permissions of the OBS bucket to ensure that the account has access to the OBS bucket mentioned in the error message.

If not, contact the OBS bucket administrator to add access permissions of the bucket.
