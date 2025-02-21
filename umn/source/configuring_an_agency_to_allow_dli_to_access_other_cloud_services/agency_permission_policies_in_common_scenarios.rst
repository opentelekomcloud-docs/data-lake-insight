:original_name: dli_01_0617.html

.. _dli_01_0617:

Agency Permission Policies in Common Scenarios
==============================================

This section provides agency permission policies for common scenarios, which can be used to configure agency permission policies when you customize your permissions. The "Resource" in the agency policy should be replaced according to specific needs.

.. _dli_01_0617__section1320218173917:

Data Cleanup Agency Permission Configuration
--------------------------------------------

Application scenario: Data cleanup agency, which is used to clean up data according to the lifecycle of a table and clean up lakehouse table data. You need to create an agency and customize permissions for it. However, the agency name is fixed to **dli_data_clean_agency**.

.. note::

   Set the authorization scope of an agency as follows:

   -  For an OBS agency, select **Global services**.
   -  For a DLI agency, select **Region-specific projects**.

.. code-block::

   {
       "Version": "1.1",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "obs:object:GetObject",
                   "obs:object:DeleteObject",
                   "obs:bucket:HeadBucket",
                   "obs:bucket:ListBucket",
                   "obs:object:PutObject"
               ]
           }
       ]
   }

.. code-block::

   {
       "Version": "1.1",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "dli:table:showPartitions",
                   "dli:table:select",
                   "dli:table:dropTable",
                   "dli:table:alterTableDropPartition"
               ]
           }
       ]
   }

.. _dli_01_0617__section02775191915:

Permission Policies for Accessing and Using OBS
-----------------------------------------------

Application scenario: For DLI Flink jobs, the permissions include downloading OBS objects, obtaining OBS/GaussDB(DWS) data sources (foreign tables), transferring logs, using savepoints, and enabling checkpointing. For DLI Spark jobs, the permissions allow downloading OBS objects and reading/writing OBS foreign tables.

.. code-block::

   {
       "Version": "1.1",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "obs:bucket:GetBucketPolicy",
                   "obs:bucket:GetLifecycleConfiguration",
                   "obs:bucket:GetBucketLocation",
                   "obs:bucket:ListBucketMultipartUploads",
                   "obs:bucket:GetBucketLogging",
                   "obs:object:GetObjectVersion",
                   "obs:bucket:GetBucketStorage",
                   "obs:bucket:GetBucketVersioning",
                   "obs:object:GetObject",
                   "obs:object:GetObjectVersionAcl",
                   "obs:object:DeleteObject",
                   "obs:object:ListMultipartUploadParts",
                   "obs:bucket:HeadBucket",
                   "obs:bucket:GetBucketAcl",
                   "obs:bucket:GetBucketStoragePolicy",
                   "obs:object:AbortMultipartUpload",
                   "obs:object:DeleteObjectVersion",
                   "obs:object:GetObjectAcl",
                   "obs:bucket:ListBucketVersions",
                   "obs:bucket:ListBucket",
                   "obs:object:PutObject"
               ],
               "Resource": [
                   "OBS:*:*:bucket:bucketName",// Replace bucketName with the actual bucket name.
                   "OBS:*:*:object:*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "obs:bucket:ListAllMyBuckets"
               ]
           }
       ]
   }

.. _dli_01_0617__section1943510461382:

Permission to Use DEW's Encryption Function
-------------------------------------------

Application scenario: DLI Flink and Spark jobs use DEW-CSMS' secret management.

.. code-block::

   {
       "Version": "1.1",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "csms:secretVersion:get",
                   "csms:secretVersion:list",
                   "kms:dek:decrypt"
               ]
           }
       ]
   }

.. _dli_01_0617__section1563832810618:

Permission to Access DLI Catalog Metadata
-----------------------------------------

Application scenario: DLI Flink and Spark jobs are authorized to access DLI metadata.

.. code-block::

   {
       "Version": "1.1",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "dli:table:showPartitions",
                   "dli:table:alterTableAddPartition",
                   "dli:table:alterTableAddColumns",
                   "dli:table:alterTableRenamePartition",
                   "dli:table:delete",
                   "dli:column:select",
                   "dli:database:dropFunction",
                   "dli:table:insertOverwriteTable",
                   "dli:table:describeTable",
                   "dli:database:explain",
                   "dli:table:insertIntoTable",
                   "dli:database:createDatabase",
                   "dli:table:alterView",
                   "dli:table:showCreateTable",
                   "dli:table:alterTableRename",
                   "dli:table:compaction",
                   "dli:database:displayAllDatabases",
                   "dli:database:dropDatabase",
                   "dli:table:truncateTable",
                   "dli:table:select",
                   "dli:table:alterTableDropColumns",
                   "dli:table:alterTableSetProperties",
                   "dli:database:displayAllTables",
                   "dli:database:createFunction",
                   "dli:table:alterTableChangeColumn",
                   "dli:database:describeFunction",
                   "dli:table:showSegments",
                   "dli:database:createView",
                   "dli:database:createTable",
                   "dli:table:showTableProperties",
                   "dli:database:showFunctions",
                   "dli:database:displayDatabase",
                   "dli:table:alterTableRecoverPartition",
                   "dli:table:dropTable",
                   "dli:table:update",
                   "dli:table:alterTableDropPartition"
               ]
           }
       ]
   }
