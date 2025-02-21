:original_name: dli_03_0188.html

.. _dli_03_0188:

Why Does a Spark Job Fail to Execute with an Abnormal Access Directory Error When Accessing Files in SFTP?
==========================================================================================================

Spark jobs cannot access SFTP. Upload the files you want to access to OBS and then you can analyze the data using Spark jobs.

#. Upload data to an OBS bucket: Upload data stored in SFTP to an OBS bucket using the OBS management console or command-line tools.
#. Configure the Spark job: Configure the Spark job to access data stored in OBS.
#. Submit the Spark job: After completing the job writing, submit and execute the job.
