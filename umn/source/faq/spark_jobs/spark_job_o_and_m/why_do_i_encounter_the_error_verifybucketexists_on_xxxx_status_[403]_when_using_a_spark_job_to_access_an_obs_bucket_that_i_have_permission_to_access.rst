:original_name: dli_03_0164.html

.. _dli_03_0164:

Why Do I Encounter the Error "verifyBucketExists on XXXX: status [403]" When Using a Spark Job to Access an OBS Bucket That I Have Permission to Access?
========================================================================================================================================================

This error message may be due to the OBS bucket being set as the DLI log bucket, which cannot be used for other purposes.

You can follow these steps to check:

#. Check if the OBS bucket has been set as the DLI log bucket.

   In the left navigation pane of the DLI management console, choose **Global Configuration** > **Job Configurations**. On the displayed page, check if the OBS bucket has been set as the DLI log bucket.

#. Check if the bucket is used for other purposes.

   If so, you can modify the job configuration on the DLI management console and select another OBS bucket that is not being used for DLI log storage.
