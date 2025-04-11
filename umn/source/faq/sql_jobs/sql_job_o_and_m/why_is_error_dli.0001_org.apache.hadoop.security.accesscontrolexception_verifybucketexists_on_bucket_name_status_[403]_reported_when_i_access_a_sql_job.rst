:original_name: dli_03_0209.html

.. _dli_03_0209:

Why Is Error "DLI.0001: org.apache.hadoop.security.AccessControlException: verifyBucketExists on {{bucket name}}: status [403]" Reported When I Access a SQL Job?
=================================================================================================================================================================

Symptom
-------

Error message "DLI.0001: org.apache.hadoop.security.AccessControlException: verifyBucketExists on {{bucket name}}: status [403]" is reported when a SQL job is Accessed.

Solution
--------

Check the permissions of the OBS bucket to ensure that the account has access to the OBS bucket mentioned in the error message.

If not, contact the OBS bucket administrator to add access permissions of the bucket.
