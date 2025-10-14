:original_name: dli_09_0191.html

.. _dli_09_0191:

Writing Data to OBS Using Flink Jar
===================================

Overview
--------

DLI allows you to use a custom JAR package to run Flink jobs and write data to OBS. This section describes how to write processed Kafka data to OBS. You need to modify the parameters in the example Java code based on site requirements.

Environment Preparations
------------------------

Development tools such as IntelliJ IDEA and other development tools, JDK, and Maven have been installed and configured.

.. note::

   -  For details about how to configure the pom.xml file of the Maven project, see "POM file configurations" in :ref:`Java Example Code <dli_09_0191__section865362419491>`.
   -  Ensure that you can access the public network in the local compilation environment.

Constraints
-----------

-  In the left navigation pane of the DLI console, choose **Global Configuration** > **Service Authorization**. On the page displayed, select **Tenant Administrator(Global service)** and click **Update**.
-  The bucket to which data is written must be an OBS bucket created by a main account.

.. _dli_09_0191__section865362419491:

Java Example Code
-----------------

-  **POM** file configurations

   .. code-block::

      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
          <parent>
              <artifactId>Flink-demo</artifactId>
              <version>1.0-SNAPSHOT</version>
          </parent>
          <modelVersion>4.0.0</modelVersion>

          <artifactId>flink-kafka-to-obs</artifactId>

          <properties>
              <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
              <!--Flink version-->
              <flink.version>1.12.2</flink.version>
              <!--JDK version -->
              <java.version>1.8</java.version>
              <!--Scala 2.11 -->
              <scala.binary.version>2.11</scala.binary.version>
              <slf4j.version>2.13.3</slf4j.version>
              <log4j.version>2.10.0</log4j.version>
              <maven.compiler.source>8</maven.compiler.source>
              <maven.compiler.target>8</maven.compiler.target>
          </properties>

          <dependencies>
              <!-- flink -->
              <dependency>
                  <groupId>org.apache.flink</groupId>
                  <artifactId>flink-java</artifactId>
                  <version>${flink.version}</version>
                  <scope>provided</scope>
              </dependency>
              <dependency>
                  <groupId>org.apache.flink</groupId>
                  <artifactId>flink-streaming-java_${scala.binary.version}</artifactId>
                  <version>${flink.version}</version>
                  <scope>provided</scope>
              </dependency>
              <dependency>
                  <groupId>org.apache.flink</groupId>
                  <artifactId>flink-statebackend-rocksdb_2.11</artifactId>
                  <version>${flink.version}</version>
                  <scope>provided</scope>
              </dependency>

              <!--  kafka  -->
              <dependency>
                  <groupId>org.apache.flink</groupId>
                  <artifactId>flink-connector-kafka_2.11</artifactId>
                  <version>${flink.version}</version>
              </dependency>

              <!--  logging  -->
              <dependency>
                  <groupId>org.apache.logging.log4j</groupId>
                  <artifactId>log4j-slf4j-impl</artifactId>
                  <version>${slf4j.version}</version>
                  <scope>provided</scope>
              </dependency>
              <dependency>
                  <groupId>org.apache.logging.log4j</groupId>
                  <artifactId>log4j-api</artifactId>
                  <version>${log4j.version}</version>
                  <scope>provided</scope>
              </dependency>
              <dependency>
                  <groupId>org.apache.logging.log4j</groupId>
                  <artifactId>log4j-core</artifactId>
                  <version>${log4j.version}</version>
                  <scope>provided</scope>
              </dependency>
              <dependency>
                  <groupId>org.apache.logging.log4j</groupId>
                  <artifactId>log4j-jcl</artifactId>
                  <version>${log4j.version}</version>
                  <scope>provided</scope>
              </dependency>
          </dependencies>

          <build>
              <plugins>
                  <plugin>
                      <groupId>org.apache.maven.plugins</groupId>
                      <artifactId>maven-assembly-plugin</artifactId>
                      <version>3.3.0</version>
                      <executions>
                          <execution>
                              <phase>package</phase>
                              <goals>
                                  <goal>single</goal>
                              </goals>
                          </execution>
                      </executions>
                      <configuration>
                          <archive>
                              <manifest>
                                  <mainClass>com.dli.FlinkKafkaToObsExample</mainClass>
                              </manifest>
                          </archive>
                          <descriptorRefs>
                              <descriptorRef>jar-with-dependencies</descriptorRef>
                          </descriptorRefs>
                      </configuration>
                  </plugin>
              </plugins>
              <resources>
                  <resource>
                      <directory>../main/config</directory>
                      <filtering>true</filtering>
                      <includes>
                          <include>**/*.*</include>
                      </includes>
                  </resource>
              </resources>
          </build>
      </project>

-  Example code

   .. code-block::

      import org.apache.flink.api.common.serialization.SimpleStringEncoder;
      import org.apache.flink.api.common.serialization.SimpleStringSchema;
      import org.apache.flink.api.java.utils.ParameterTool;
      import org.apache.flink.contrib.streaming.state.RocksDBStateBackend;
      import org.apache.flink.core.fs.Path;
      import org.apache.flink.runtime.state.filesystem.FsStateBackend;
      import org.apache.flink.streaming.api.datastream.DataStream;
      import org.apache.flink.streaming.api.environment.CheckpointConfig;
      import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
      import org.apache.flink.streaming.api.functions.sink.filesystem.StreamingFileSink;
      import org.apache.flink.streaming.api.functions.sink.filesystem.bucketassigners.DateTimeBucketAssigner;
      import org.apache.flink.streaming.api.functions.sink.filesystem.rollingpolicies.OnCheckpointRollingPolicy;
      import org.apache.flink.streaming.connectors.kafka.FlinkKafkaConsumer;
      import org.apache.kafka.clients.consumer.ConsumerConfig;
      import org.slf4j.Logger;
      import org.slf4j.LoggerFactory;

      import java.util.Properties;

      /**
       * @author xxx
       * @date 6/26/21
       */
      public class FlinkKafkaToObsExample {
          private static final Logger LOG = LoggerFactory.getLogger(FlinkKafkaToObsExample.class);

          public static void main(String[] args) throws Exception {
              LOG.info("Start Kafka2OBS Flink Streaming Source Java Demo.");
              ParameterTool params = ParameterTool.fromArgs(args);
              LOG.info("Params: " + params.toString());

              // Kafka connection address
              String bootstrapServers;
              // Kafka consumer group
              String kafkaGroup;
              // Kafka topic
              String kafkaTopic;
              // Consumption policy. This policy is used only when the partition does not have a checkpoint or the checkpoint expires.
              // If a valid checkpoint exists, consumption continues from this checkpoint.
              // When the policy is set to LATEST, the consumption starts from the latest data. This policy will ignore the existing data in the stream.
              // When the policy is set to EARLIEST, the consumption starts from the earliest data. This policy will obtain all valid data in the stream.
              String offsetPolicy;
              // OBS file output path, in the format of obs://bucket/path.
              String outputPath;
              // Checkpoint output path, in the format of obs://bucket/path.
              String checkpointPath;

              bootstrapServers = params.get("bootstrap.servers", "xxxx:9092,xxxx:9092,xxxx:9092");
              kafkaGroup = params.get("group.id", "test-group");
              kafkaTopic = params.get("topic", "test-topic");
              offsetPolicy = params.get("offset.policy", "earliest");
              outputPath = params.get("output.path", "obs://bucket/output");
             checkpointPath = params.get("checkpoint.path", "obs://bucket/checkpoint");

              try {
                  //Create an execution environment.
                  StreamExecutionEnvironment streamEnv = StreamExecutionEnvironment.getExecutionEnvironment();
                  streamEnv.setParallelism(4);
                  RocksDBStateBackend rocksDbBackend = new RocksDBStateBackend(checkpointPath, true);
                  RocksDBStateBackend rocksDbBackend = new RocksDBStateBackend(new FsStateBackend(checkpointPath), true);
                  streamEnv.setStateBackend(rocksDbBackend);
                  // Enable Flink checkpointing mechanism. If enabled, the offset information will be synchronized to Kafka.
                  streamEnv.enableCheckpointing(300000);
                  // Set the minimum interval between two checkpoints.
                  streamEnv.getCheckpointConfig().setMinPauseBetweenCheckpoints(60000);
                  // Set the checkpoint timeout duration.
                  streamEnv.getCheckpointConfig().setCheckpointTimeout(60000);
                  // Set the maximum number of concurrent checkpoints.
                  streamEnv.getCheckpointConfig().setMaxConcurrentCheckpoints(1);
                  // Retain checkpoints when a job is canceled.
                  streamEnv.getCheckpointConfig().enableExternalizedCheckpoints(
                          CheckpointConfig.ExternalizedCheckpointCleanup.RETAIN_ON_CANCELLATION);

                  // Source: Connect to the Kafka data source.
                  Properties properties = new Properties();
                  properties.setProperty("bootstrap.servers", bootstrapServers);
                  properties.setProperty("group.id", kafkaGroup);
                  properties.setProperty(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, offsetPolicy);
                  String topic = kafkaTopic;

                  // Create a Kafka consumer.
                  FlinkKafkaConsumer<String> kafkaConsumer =
                          new FlinkKafkaConsumer<>(topic, new SimpleStringSchema(), properties);
                  /**
                   * Read partitions from the offset submitted by the consumer group (specified by group.id in the consumer attribute) in Kafka brokers.
                   * If the partition offset cannot be found, set it by using the auto.offset.reset parameter.
                   * For details, see https://ci.apache.org/projects/flink/flink-docs-release-1.13/zh/docs/connectors/datastream/kafka/.
                   */
                  kafkaConsumer.setStartFromGroupOffsets();

                  // Add Kafka to the data source.
                  DataStream<String> stream = streamEnv.addSource(kafkaConsumer).setParallelism(3).disableChaining();

                  // Create a file output stream.
                  final StreamingFileSink<String> sink = StreamingFileSink
                          // Specify the file output path and row encoding format.
                          .forRowFormat(new Path(outputPath), new SimpleStringEncoder<String>("UTF-8"))
                          // Specify the file output path and bulk encoding format. Files are output in parquet format.
                          //.forBulkFormat(new Path(outputPath), ParquetAvroWriters.forGenericRecord(schema))
                          // Specify a custom bucket assigner.
                          .withBucketAssigner(new DateTimeBucketAssigner<>())
                          // Specify the rolling policy.
                          .withRollingPolicy(OnCheckpointRollingPolicy.build())
                          .build();

                  // Add sink for DIS Consumer data source
                  stream.addSink(sink).disableChaining().name("obs");

                  // stream.print();
                  streamEnv.execute();
              } catch (Exception e) {
                  LOG.error(e.getMessage(), e);
              }
          }
      }

   .. table:: **Table 1** Parameter description

      +-------------------+----------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
      | Parameter         | Description                            | Example                                                                                                                      |
      +===================+========================================+==============================================================================================================================+
      | bootstrap.servers | Kafka connection address               | *IP address of the Kafka service* 1:9092, *IP address of the Kafka service* 2:9092, *IP address of the Kafka service* 3:9092 |
      +-------------------+----------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
      | group.id          | Kafka consumer group                   | **test-group**                                                                                                               |
      +-------------------+----------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
      | topic             | Kafka consumption topic                | **test-topic**                                                                                                               |
      +-------------------+----------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
      | offset.policy     | Kafka offset policy                    | **earliest**                                                                                                                 |
      +-------------------+----------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
      | output.path       | OBS path to which data will be written | obs://bucket/output                                                                                                          |
      +-------------------+----------------------------------------+------------------------------------------------------------------------------------------------------------------------------+
      | checkpoint.path   | Checkpoint OBS path                    | obs://bucket/checkpoint                                                                                                      |
      +-------------------+----------------------------------------+------------------------------------------------------------------------------------------------------------------------------+

Compiling and Running the Application
-------------------------------------

After the application is developed, upload the JAR package to DLI by referring to :ref:`Flink Jar Job Examples <dli_09_0150>` and check whether related data exists in the OBS path.
