:original_name: dli_09_0121.html

.. _dli_09_0121:

Performing Authentication
=========================

Scenario
--------

You need to be authenticated when using JDBC to create DLI driver connections.

Procedure
---------

Currently, the JDBC supports authentication using the Access Key/Secret Key (AK/SK) or token.

-  (Recommended) Generating an AK/SK

   #. Log in to the DLI management console.
   #. Click the username in the upper right corner and select **My Credentials** from the drop-down list.
   #. On the displayed **My Credentials** page, click **Access Keys**. By default, the **Project List** page is displayed.
   #. Click **Add Access Key**. In the displayed **Add Access Key** dialog box, specify **Login Password** and **SMS Verification Code**.
   #. Click **OK** to download the certificate.
   #. After the certificate is downloaded, you can obtain the AK and SK information in the **credentials** file.

   .. note::

      Hard coding AKs and SKs or storing them in code in plaintext poses significant security risks. You are advised to store them in encrypted form in configuration files or environment variables and decrypt them when needed to ensure security.

-  Obtain the token.

   When using token authentication, you need to obtain the user token and configure the token information in the JDBC connection parameters. You can obtain the token as follows:

   #. Send **POST https://<IAM_Endpoint>/v3/auth/tokens**. To obtain the IAM endpoint, contact the administrator to obtain the region and endpoint information.

      An example request message is as follows:

      .. note::

         Replace content in italic in the sample code with the actual values. For details, see .

      .. code-block::

         {
           "auth": {
             "identity": {
               "methods": [
                 "password"
               ],
               "password": {
                 "user": {
                   "name": "username",
                   "password": "password",
                   "domain": {
                     "name": "domainname"
                   }
                 }
               }
             },
             "scope": {
               "project": {
                 "id": "0aa253a31a2f4cfda30eaa073fee6477" //Assume that project_id is 0aa253a31a2f4cfda30eaa073fee6477.
               }
             }
           }
         }

   #. After the request is processed, the value of **X-Subject-Token** in the response header is the token value.
