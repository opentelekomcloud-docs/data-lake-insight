:original_name: dli_03_0038.html

.. _dli_03_0038:

How Do I Configure Checkpoints for Flink Jar Jobs and Save the Checkpoints to OBS?
==================================================================================

The procedure is as follows:

#. Add the following code to the JAR file code of the Flink Jar job:

   .. code-block::

      // Configure the pom file on which the StreamExecutionEnvironment depends.
      StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

              env.getCheckpointConfig().setCheckpointingMode(CheckpointingMode.EXACTLY_ONCE);
              env.getCheckpointConfig().setCheckpointInterval(40000);
              env.getCheckpointConfig().enableExternalizedCheckpoints(CheckpointConfig.ExternalizedCheckpointCleanup.RETAIN_ON_CANCELLATION);
              RocksDBStateBackend rocksDbBackend = new RocksDBStateBackend(new FsStateBackend("obs://${bucket}/jobs/checkpoint/my_jar"), false);
              rocksDbBackend.setOptions(new OptionsFactory() {
                  @Override
                  public DBOptions createDBOptions(DBOptions currentOptions) {
                      return currentOptions
                              .setMaxLogFileSize(64 * 1024 * 1024)
                              .setKeepLogFileNum(3);
                  }

                  @Override
                  public ColumnFamilyOptions createColumnOptions(ColumnFamilyOptions currentOptions) {
                      return currentOptions;
                  }
              });
              env.setStateBackend(rocksDbBackend);

   .. note::

      The preceding code saves the checkpoint to the **${bucket}** bucket in **jobs/checkpoint/my_jar** path every 40 seconds in **EXACTLY_ONCE** mode.

      Pay attention to the checkpoint storage path. Generally, the checkpoint is stored in the OBS bucket. The path format is as follows:

      -  Path format: **obs://$**\ *{bucket}*\ **/**\ *xxx*\ **/**\ *xxx*\ **/**\ *xxx*

      -  Add the following configuration to the POM file for the packages on which the StreamExecutionEnvironment depends:

         .. code-block::

            <dependency>
                        <groupId>org.apache.flink</groupId>
                        <artifactId>flink-streaming-java_${scala.binary.version}</artifactId>
                        <version>${flink.version}</version>
                        <scope>provided</scope>
            </dependency>

#. Configure **Runtime Configuration** and **Restore Job from Checkpoint** for a DLI Flink Jar job.

   -  Constraints on parameter optimization

      a. In the left navigation pane of the DLI console, choose **Global Configuration** > **Service Authorization**. On the page displayed, select **Tenant Administrator(Global service)** and click **Update**.
      b. The bucket to which data is written must be an OBS bucket created by a main account.

   -  Configuring **Restore Job from Checkpoint**

      a. Select **Auto Restart upon Exception**.

      b. Select **Restore Job from Checkpoint** and set the **Checkpoint Path**.

         The checkpoint path is the same as that you set in JAR file code. The format is as follows:

         -  ${bucket}/xxx/xxx/xxx

         -  Example

            If the path in the JAR file is **obs://mybucket/jobs/checkpoint/jar-3**,

            Set **Checkpoint Path** to **mybucket/jobs/checkpoint/jar-3**.

      .. note::

         -  The checkpoint path for each Flink Jar job must be unique. Otherwise, data cannot be restored.
         -  DLI can access files in the checkpoint path only after DLI is authorized to access the OBS bucket.

#. Check whether the job is restored from the checkpoint.
