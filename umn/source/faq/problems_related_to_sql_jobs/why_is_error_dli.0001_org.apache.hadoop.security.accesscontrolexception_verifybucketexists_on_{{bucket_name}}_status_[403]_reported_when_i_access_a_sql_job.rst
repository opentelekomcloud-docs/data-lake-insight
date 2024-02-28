:original_name: dli_03_0209.html

.. _dli_03_0209:

Why Is Error "DLI.0001: org.apache.hadoop.security.AccessControlException: verifyBucketExists on {{bucket name}}: status [403]" Reported When I Access a SQL Job?
=================================================================================================================================================================

Symptom
-------

Error message "DLI.0001: org.apache.hadoop.security.AccessControlException: verifyBucketExists on {{bucket name}}: status [403]" is reported when a SQL job is Accessed.

Solution
--------

Your account does not have the permission to access the OBS bucket where the foreign table is located. Obtain the OBS permission and try the query again.
