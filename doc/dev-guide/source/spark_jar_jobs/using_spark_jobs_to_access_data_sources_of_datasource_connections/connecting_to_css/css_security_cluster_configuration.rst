:original_name: dli_09_0189.html

.. _dli_09_0189:

CSS Security Cluster Configuration
==================================

.. _dli_09_0189__section4741546145716:

Preparations
------------

The Elasticsearch 6.5.4 and later versions provided by CSS provides the security settings. Once the function is enabled, CSS provides identity authentication, authorization, and encryption for users. Before connecting DLI to the CSS security cluster, you need to perform certain preparations.

#. Select CSS Elasticsearch 6.5.4 or a later cluster version, create a CSS security cluster, and download the security cluster certificate (**CloudSearchService.cer**).

   a. Log in to the CSS management console, click **Clusters**, and select the cluster for which you want to create a datasource connection.
   b. Click **Download Certificate** next to **Security Mode** to download the security certificate.

#. Use keytool to generate the **keystore** and **truststore** files.

   a. Security certificate **CloudSearchService.cer** of the security cluster is required when you use keytool to generate the keystore and truststore files. You can set other keytool parameters as required.

      #. Open the cmd command window and run the following command to generate a **keystore** file that contains a private key:

         .. code-block::

            keytool -genkeypair -alias certificatekey -keyalg RSA -keystore transport-keystore.jks

      #. After the **keystore** and **truststore** files are generated using keytool, you can view the **transport-keystore.jks** file in the folder. Run the following command to verify the **keystore** file and certificate information:

         .. code-block::

            keytool -list -v -keystore transport-keystore.jks

         After you enter the correct keystore password, the corresponding information is displayed.

      #. Run the following commands to create the **truststore.jks** file and verify it:

         .. code-block::

            keytool -import -alias certificatekey -file CloudSearchService.cer  -keystore truststore.jks
            keytool -list -v -keystore truststore.jks

   b. Upload the generated **keystore** and **truststore** files to an OBS bucket.

CSS Security Cluster Parameter Configuration
--------------------------------------------

For details about the parameters, see :ref:`Table 1 <dli_09_0061__en-us_topic_0190067468_table569314388144>`. This part describes the precautions for configuring the connection parameters of the CSS security cluster.

.. code-block::

   .option("es.net.http.auth.user", "admin") .option("es.net.http.auth.pass", "***")

The parameters are the identity authentication account and password, which are also the account and password for logging in to Kibana.

.. code-block::

   .option("es.net.ssl", "true")

-  If HTTPS access is enabled for the CSS security cluster, set this parameter to **true** and then set parameters such as the security certificate and file address.
-  If HTTPS access is not enabled for the CSS security cluster, set this parameter to **false**. In this case, you do not need to set parameters such as the security certificate and file address.

.. code-block::

   .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")
   .option("es.net.ssl.keystore.pass", "***")

Set the location of the **keystore.jks** file and the key for accessing the file. Place the **keystore.jks** file generated in :ref:`Preparations <dli_09_0189__section4741546145716>` in the OBS bucket, and then enter the AK, SK, and location of the **keystore.jks** file. Enter the key for accessing the file in **es.net.ssl.keystore.pass**.

.. code-block::

   .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")
   .option("es.net.ssl.truststore.pass", "***")

The parameters in the **truststore.jks** file are basically the same as those in the **keystore.jks** file. You can refer to the preceding procedure to set parameters.
