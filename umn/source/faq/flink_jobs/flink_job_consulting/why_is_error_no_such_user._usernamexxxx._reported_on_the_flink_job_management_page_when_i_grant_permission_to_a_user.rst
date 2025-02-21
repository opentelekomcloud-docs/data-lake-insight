:original_name: dli_03_0160.html

.. _dli_03_0160:

Why Is Error "No such user. userName:xxxx." Reported on the Flink Job Management Page When I Grant Permission to a User?
========================================================================================================================

Symptom
-------

Choose **Job Management** > **Flink Jobs**. In the **Operation** column of the target job, choose **More** > **Permissions**. When a new user is authorized, **No such user. userName:xxxx.** is displayed.

Solution
--------

The issue is caused by the system's failure to identify new user information.

Perform the following steps:

#. Check whether the username exists.
#. If the username exists, relog in to the management console to grant permissions.
