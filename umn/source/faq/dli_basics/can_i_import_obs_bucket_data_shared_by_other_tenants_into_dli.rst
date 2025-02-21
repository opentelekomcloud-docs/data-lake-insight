:original_name: dli_03_0129.html

.. _dli_03_0129:

Can I Import OBS Bucket Data Shared by Other Tenants into DLI?
==============================================================

DLI supports importing data from OBS buckets shared by IAM users under the same tenant, but not from OBS buckets shared by other tenants.

This ensures data security and isolation.

For scenarios that require cross-tenant data sharing and analysis, you are advised to first mask the data and upload it to an OBS bucket before performing data analysis. After the analysis is completed, delete the temporary data in the OBS bucket in a timely manner to ensure data security.
