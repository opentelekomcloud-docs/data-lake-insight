:original_name: dli_03_0173.html

.. _dli_03_0173:

Why Is Error "org.apache.hadoop.fs.obs.OBSIOException" Reported When I Run DLI SQL Scripts on DataArts Studio?
==============================================================================================================

Symptom
-------

When you run a DLI SQL script on DataArts Studio, the log shows that the statements fail to be executed. The error information is as follows:

.. code-block::

   DLI.0999: RuntimeException: org.apache.hadoop.fs.obs.OBSIOException: initializing on obs://xxx.csv: status [-1] - request id
   [null] - error code [null] - error message [null] - trace :com.obs.services.exception.ObsException: OBS servcie Error Message. Request Error:
   ...
   Cause by: ObsException: com.obs.services.exception.ObsException: OBSs servcie Error Message. Request Error: java.net.UnknownHostException: xxx: Name or service not known

Possible Causes
---------------

When you execute a DLI SQL script for the first time, you did not agree to the privacy agreement on the DLI console. As a result, the error is reported when the SQL script is executed on DataArts Studio.

Solution
--------

#. Log in to the DLI console, click **SQL Editor** from the navigation pane. On the displayed page, enter an SQL statement in the editing window, for example, **select 1**.
#. In the displayed **Privacy Agreement** dialog box, agree to the terms.

   .. note::

      You only need to agree to the privacy agreement when it is your first time to execute the statements.

#. Run the DLI SQL script on DataArts Studio again. The script will run properly.
