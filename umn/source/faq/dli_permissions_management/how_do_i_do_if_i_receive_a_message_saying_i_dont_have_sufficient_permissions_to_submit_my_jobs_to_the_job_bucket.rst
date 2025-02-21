:original_name: dli_03_0225.html

.. _dli_03_0225:

How Do I Do If I Receive a Message Saying I Don't Have Sufficient Permissions to Submit My Jobs to the Job Bucket?
==================================================================================================================

Symptom
-------

Despite configuring a job bucket and authorizing DLI to access it, I still receive an error message stating that DLI is not authorized to access the bucket when attempting to submit a job.

Possible Causes
---------------

To use a DLI job bucket, ensure that necessary permissions for the bucket have been configured.

To ensure that DLI can perform necessary operations, check the bucket policy of the DLI job bucket on the OBS management console and verify that it contains the required authorization information.

Make sure that there are no policies explicitly denying DLI access to the bucket. IAM policies prioritize deny permissions over allow permissions, which means that even if there are allow permissions, the presence of deny permissions will result in authorization failure.

Solution
--------

#. Find the DLI job bucket on the OBS management console.

#. .. _dli_03_0225__li137621171113:

   View the policy of the bucket you select.

   The authorization information required for DLI Flink jobs to use the bucket is as follows: *domainId* and *userId* are the DLI account and sub-account, respectively, *bucketName* is the user's bucket name, and *timeStamp* is the timestamp when the policy was created.

   .. code-block::

      {
          "Statement": [
              {
                  "Effect": "Allow",
                  "Principal": {
                      "ID": [
                          "domain/domainId:user/userId"
                      ]
                  },
                  "Action": [
                      "GetObject",
                      "GetObjectVersion",
                      "PutObject",
                      "DeleteObject",
                      "DeleteObjectVersion",
                      "ListMultipartUploadParts",
                      "AbortMultipartUpload",
                      "GetObjectAcl",
                      "GetObjectVersionAcl"
                  ],
                  "Resource": [
                      "bucketName/*"
                  ],
                  "Sid": "Untitled bucket policy-Timestamp-0"
              },
              {
                  "Effect": "Allow",
                  "Principal": {
                      "ID": [
                          "domain/domainId:user/userId "
                      ]
                  },
                  "Action": [
                      "HeadBucket",
                      "ListBucket",
                      "ListBucketVersions",
                      "ListBucketMultipartUploads",
                      "GetBucketAcl",
                      "GetBucketLocation",
                      "GetBucketLogging",
                      "GetLifecycleConfiguration"
                  ],
                  "Resource": [
                      " bucketName "
                  ],
                  "Sid": "Untitled bucket policy-Timestamp-1"
              }
          ]
      }

#. Check the following permission content on the management console and verify if the policy name matches the one in :ref:`2 <dli_03_0225__li137621171113>`.

   -  **Effect**: Select **Allow**.
   -  **Resources**: Authorize buckets and objects as needed.
   -  **Actions**: Select those configured for **Action** in :ref:`2 <dli_03_0225__li137621171113>`.

   **Common check items:**

   -  Check if any deny operations are configured for all accounts, and if these operations are the required authorization operations for DLI.
   -  Check if any deny operations are configured for the authorized users of DLI, and if these operations are the required authorization operations for DLI.
