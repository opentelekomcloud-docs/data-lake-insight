:original_name: dli_03_0044.html

.. _dli_03_0044:

Can I Upload Configuration Files for Flink Jar Jobs?
====================================================

Uploading a Configuration File for a Flink Jar Job
--------------------------------------------------

You can upload configuration files for custom jobs (Jar).

#. Upload the configuration file to DLI through **Package Management**.
#. In the **Other Dependencies** area of the Flink Jar job, select the created DLI package.
#. Load the file through **ClassName.class.getClassLoader().getResource("userData/fileName")** in the code. In the file name, **fileName** indicates the name of the file to be accessed, and **ClassName** indicates the name of the class that needs to access the file.

Using a Configuration File
--------------------------

-  Solution 1: Load the file content to the memory in the **main** function and broadcast the content to each taskmanager. This method is applicable to the scenario where a small number of variables need to be loaded in advance.

-  Solution 2: Load the file when initializing the operator in **open**. A relative or absolute path can be used.

   Take Kafka sink as an example. Two files (**userData/kafka-sink.conf** and **userData/client.truststore.jks**) need to be loaded.

   -  **Example of using a relative path:**

      .. code-block::

         Relative path: confPath = userData/kafka-sink.conf
         @Override
         public void open(Configuration parameters) throws Exception {
             super.open(parameters);
             initConf();
             producer = new KafkaProducer<>(props);
         }
         private void initConf() {
             try {
                 URL url = DliFlinkDemoDis2Kafka.class.getClassLoader().getResource(confPath);
                 if (url != null) {
                     LOGGER.info("kafka main-url: " + url.getFile());
                 } else {
                     LOGGER.info("kafka url error......");
                 }
                 InputStream inputStream = new BufferedInputStream(new FileInputStream(new File(url.getFile()).getAbsolutePath()));
                 props.load(new InputStreamReader(inputStream, "UTF-8"));
                 topic = props.getProperty("topic");
                 partition = Integer.parseInt(props.getProperty("partition"));
                 vaildProps();
             } catch (Exception e) {
                 LOGGER.info("load kafka conf failed");
                 e.printStackTrace();
             }
         }


      .. figure:: /_static/images/en-us_image_0000001716012053.png
         :alt: **Figure 1** Example relative path configuration

         **Figure 1** Example relative path configuration

   -  **Example of using an absolute path:**

      .. code-block::

         Absolute path: confPath = userData/kafka-sink.conf / path = /opt/data1/hadoop/tmp/usercache/omm/appcache/application_xxx_0015/container_xxx_0015_01_000002/userData/client.truststore.jks
         @Override
         public void open(Configuration parameters) throws Exception {
             super.open(parameters);
             initConf();
             String path = DliFlinkDemoDis2Kafka.class.getClassLoader().getResource("userData/client.truststore.jks").getPath();
             LOGGER.info("kafka abs path " + path);
             props.setProperty("ssl.truststore.location", path);
             producer = new KafkaProducer<>(props);
         }
         private void initConf() {
             try {
                 URL url = DliFlinkDemoDis2Kafka.class.getClassLoader().getResource(confPath);
                 if (url != null) {
                     LOGGER.info("kafka main-url: " + url.getFile());
                 } else {
                     LOGGER.info("kafka url error......");
                 }
                 InputStream inputStream = new BufferedInputStream(new FileInputStream(new File(url.getFile()).getAbsolutePath()));
                 props.load(new InputStreamReader(inputStream, "UTF-8"));
                 topic = props.getProperty("topic");
                 partition = Integer.parseInt(props.getProperty("partition"));
                 vaildProps();
             } catch (Exception e) {
                 LOGGER.info("load kafka conf failed");
                 e.printStackTrace();
             }
         }


      .. figure:: /_static/images/en-us_image_0000001668054032.png
         :alt: **Figure 2** Example absolute path configuration

         **Figure 2** Example absolute path configuration
