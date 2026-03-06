:original_name: dli_09_0211.html

.. _dli_09_0211:

Flink Jar Jobs Using DEW to Acquire Access Credentials for Reading and Writing Data from and to OBS
===================================================================================================

Scenario
--------

When writing output data from Flink Jar jobs to OBS, you need to configure an AK/SK for accessing OBS. To ensure the security of AK/SK data, you can use DEW and CSMS for centralized management of AK/SK. This approach effectively mitigates risks such as sensitive information leakage caused by hardcoding in programs or plaintext configurations, as well as potential business disruptions due to unauthorized access.

This section walks you through on how a Flink Jar job acquires an AK/SK to read and write data from and to OBS.

Notes and Constraints
---------------------

DEW can be used to manage access credentials only in Flink 1.15. When creating a Flink job, select version 1.15 and configure the information of the agency that allows DLI to access DEW for the job.

Prerequisites
-------------

-  A shared secret has been created on the DEW console and the secret value has been stored.
-  An agency has been created and authorized for DLI to access DEW. The agency must have been granted the following permissions:

   -  Permission of the **ShowSecretVersion** interface for querying secret versions and secret values in DEW: **csms:secretVersion:get**.
   -  Permission of the **ListSecretVersions** interface for listing secret versions in DEW: **csms:secretVersion:list**.
   -  Permission to decrypt DEW secrets: **kms:dek:decrypt**

-  To use this function, you need to configure AK/SK for all OBS buckets.

Syntax
------

On the Flink Jar job editing page, set **Runtime Configuration** as needed. The configuration information is as follows:

Different OBS buckets use different AK/SK authentication information. You can use the following configuration method to specify the AK/SK information based on the bucket. For details about the parameters, see :ref:`Table 1 <dli_09_0211__en-us_topic_0000001836634008_en-us_topic_0000001794701748_table517231215112>`.

::

   flink.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.access.key=USER_AK_CSMS_KEY
   flink.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.secret.key=USER_SK_CSMS_KEY
   flink.hadoop.fs.obs.security.provider=com.dli.provider.UserObsBasicCredentialProvider
   flink.hadoop.fs.dew.csms.secretName=CredentialName
   flink.hadoop.fs.dew.endpoint=ENDPOINT
   flink.hadoop.fs.dew.csms.version=VERSION_ID
   flink.hadoop.fs.dew.csms.cache.time.second=CACHE_TIME
   flink.dli.job.agency.name=USER_AGENCY_NAME

Parameter Description
---------------------

.. _dli_09_0211__en-us_topic_0000001836634008_en-us_topic_0000001794701748_table517231215112:

.. table:: **Table 1** Parameter descriptions

   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                                  | Mandatory   | Default Value  | Data Type   | Description                                                                                                                                                                                                                                                                                                     |
   +============================================================+=============+================+=============+=================================================================================================================================================================================================================================================================================================================+
   | flink.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.access.key | Yes         | None           | String      | *USER_BUCKET_NAME* needs to be replaced with the user's OBS bucket name.                                                                                                                                                                                                                                        |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | The value of this parameter is the key defined by the user in the CSMS shared secret. The value corresponding to the key is the user's access key ID (AK). The user must have the permission to access the bucket on OBS.                                                                                       |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.secret.key | Yes         | None           | String      | *USER_BUCKET_NAME* needs to be replaced with the user's OBS bucket name.                                                                                                                                                                                                                                        |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | The value of this parameter is the key defined by the user in the CSMS shared secret. The value corresponding to the key is the user's secret access key (SK). The user must have the permission to access the bucket on OBS.                                                                                   |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink.hadoop.fs.obs.security.provider                      | Yes         | None           | String      | OBS AK/SK authentication mechanism, which uses DEW-CSMS' secret management to obtain the AK and SK for accessing OBS.                                                                                                                                                                                           |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | The default value is **com.dli.provider.UserObsBasicCredentialProvider**.                                                                                                                                                                                                                                       |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink.hadoop.fs.dew.endpoint                               | Yes         | None           | String      | Endpoint of the DEW service to be used.                                                                                                                                                                                                                                                                         |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink.hadoop.fs.dew.projectId                              | No          | Yes            | String      | ID of the project DEW belongs to. The default value is the ID of the project where the Flink job is.                                                                                                                                                                                                            |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink.hadoop.fs.dew.csms.secretName                        | Yes         | None           | String      | Name of the shared secret in DEW's secret management.                                                                                                                                                                                                                                                           |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | Configuration example: **flink.hadoop.fs.dew.csms.secretName=**\ *secretInfo*                                                                                                                                                                                                                                   |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink.hadoop.fs.dew.csms.version                           | Yes         | Latest version | String      | Version number (secret version identifier) of the shared secret created in DEW CSMS.                                                                                                                                                                                                                            |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | If the latest version number (secret version identifier) is not specified, the system will not be able to directly retrieve the most recent version of the secret, potentially leading to application access failures or the use of outdated secrets, thereby compromising data security and service stability. |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | View the version information of the secret on the DEW management console and configure this parameter with the latest version number to ensure that applications can securely and reliably access the necessary data.                                                                                           |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | Configuration example: **flink.hadoop.fs.dew.csms.version=v1**                                                                                                                                                                                                                                                  |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink.hadoop.fs.dew.csms.cache.time.second                 | No          | 3600           | Long        | Cache duration after the CSMS shared secret is obtained during Flink job access.                                                                                                                                                                                                                                |
   |                                                            |             |                |             |                                                                                                                                                                                                                                                                                                                 |
   |                                                            |             |                |             | The unit is second. The default value is 3600 seconds.                                                                                                                                                                                                                                                          |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | flink.dli.job.agency.name                                  | Yes         | ``-``          | String      | Custom agency name.                                                                                                                                                                                                                                                                                             |
   +------------------------------------------------------------+-------------+----------------+-------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Sample Code
-----------

This section describes how to write processed DataGen data to OBS. You need to modify the parameters in the sample Java code based on site requirements.

#. Create an agency for DLI to access DEW and complete authorization.
#. Create a shared secret in DEW.

   a. Log in to the DEW management console.
   b. In the navigation pane on the left, choose **Cloud Secret Management Service** > **Secrets**.
   c. On the displayed page, click **Create Secret**. Set basic secret information.

#. Set job parameters on the DLI Flink Jar job editing page.

   -  Class name

      ::

         com.dli.demo.dew.DataGen2FileSystemSink

   -  Parameters

      ::

         --checkpoint.path obs://test/flink/jobs/checkpoint/120891/
         --output.path obs://dli/flink.db/79914/DataGen2FileSystemSink

   -  Runtime configuration:

      ::

         flink.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.access.key=USER_AK_CSMS_KEY
         flink.hadoop.fs.obs.bucket.USER_BUCKET_NAME.dew.secret.key=USER_SK_CSMS_KEY
         flink.hadoop.fs.obs.security.provider=com.dli.provider.UserObsBasicCredentialProvider
         flink.hadoop.fs.dew.csms.secretName=obsAksK
         flink.hadoop.fs.dew.endpoint=kmsendpoint
         flink.hadoop.fs.dew.csms.version=v6
         flink.hadoop.fs.dew.csms.cache.time.second=3600
         flink.dli.job.agency.name=***

#. Flink Jar job example.

   -  **Environment preparation**

      Development tools such as IntelliJ IDEA and other development tools, JDK, and Maven have been installed and configured.

      Dependency package in POM file configurations

      ::

          <properties>
                 <flink.version>1.15.0</flink.version>
             </properties>

             <dependencies>
                 <dependency>
                     <groupId>org.apache.flink</groupId>
                     <artifactId>flink-statebackend-rocksdb</artifactId>
                     <version>${flink.version}</version>
                     <scope>provided</scope>
                 </dependency>

                 <dependency>
                     <groupId>org.apache.flink</groupId>
                     <artifactId>flink-streaming-java</artifactId>
                     <version>${flink.version}</version>
                     <scope>provided</scope>
                 </dependency>

                 <!-- fastjson -->
                 <dependency>
                     <artifactId>fastjson</artifactId>
                     <version>2.0.15</version>
                 </dependency>
             </dependencies>

   -  **Example code**

      ::

         import org.apache.flink.api.common.serialization.SimpleStringEncoder;
         import org.apache.flink.api.java.utils.ParameterTool;
         import org.apache.flink.contrib.streaming.state.EmbeddedRocksDBStateBackend;
         import org.apache.flink.core.fs.Path;
         import org.apache.flink.streaming.api.datastream.DataStream;
         import org.apache.flink.streaming.api.environment.CheckpointConfig;
         import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
         import org.apache.flink.streaming.api.functions.sink.filesystem.StreamingFileSink;
         import org.apache.flink.streaming.api.functions.sink.filesystem.rollingpolicies.OnCheckpointRollingPolicy;
         import org.apache.flink.streaming.api.functions.source.ParallelSourceFunction;
         import org.slf4j.Logger;
         import org.slf4j.LoggerFactory;

         import java.time.LocalDateTime;
         import java.time.ZoneOffset;
         import java.time.format.DateTimeFormatter;
         import java.util.Random;

         public class DataGen2FileSystemSink {
             private static final Logger LOG = LoggerFactory.getLogger(DataGen2FileSystemSink.class);

             public static void main(String[] args) {
                 ParameterTool params = ParameterTool.fromArgs(args);
                 LOG.info("Params: " + params.toString());
                 try {
                     StreamExecutionEnvironment streamEnv = StreamExecutionEnvironment.getExecutionEnvironment();

                     // set checkpoint
                     String checkpointPath = params.get("checkpoint.path", "obs://bucket/checkpoint/jobId_jobName/");
                     LocalDateTime localDateTime = LocalDateTime.ofEpochSecond(System.currentTimeMillis() / 1000,
                         0, ZoneOffset.ofHours(8));
                     String dt = localDateTime.format(DateTimeFormatter.ofPattern("yyyyMMdd_HH:mm:ss"));
                     checkpointPath = checkpointPath + dt;

                     streamEnv.setStateBackend(new EmbeddedRocksDBStateBackend());
                     streamEnv.getCheckpointConfig().setCheckpointStorage(checkpointPath);
                     streamEnv.getCheckpointConfig().setExternalizedCheckpointCleanup(
                         CheckpointConfig.ExternalizedCheckpointCleanup.RETAIN_ON_CANCELLATION);
                     streamEnv.enableCheckpointing(30 * 1000);

                     DataStream<String> stream = streamEnv.addSource(new DataGen())
                         .setParallelism(1)
                         .disableChaining();

                     String outputPath = params.get("output.path", "obs://bucket/outputPath/jobId_jobName");

                     // Sink OBS
                     final StreamingFileSink<String> sinkForRow = StreamingFileSink
                         .forRowFormat(new Path(outputPath), new SimpleStringEncoder<String>("UTF-8"))
                         .withRollingPolicy(OnCheckpointRollingPolicy.build())
                         .build();

                     stream.addSink(sinkForRow);

                     streamEnv.execute("sinkForRow");
                 } catch (Exception e) {
                     LOG.error(e.getMessage(), e);
                 }
             }
         }

         class DataGen implements ParallelSourceFunction<String> {

             private boolean isRunning = true;

             private Random random = new Random();

             @Override
             public void run(SourceContext<String> ctx) throws Exception {
                 while (isRunning) {
                     JSONObject jsonObject = new JSONObject();
                     jsonObject.put("id", random.nextLong());
                     jsonObject.put("name", "Molly" + random.nextInt());
                     jsonObject.put("address", "hangzhou" + random.nextInt());
                     jsonObject.put("birthday", System.currentTimeMillis());
                     jsonObject.put("city", "hangzhou" + random.nextInt());
                     jsonObject.put("number", random.nextInt());
                     ctx.collect(jsonObject.toJSONString());
                     Thread.sleep(1000);
                 }
             }

             @Override
             public void cancel() {
                 isRunning = false;
             }
         }
