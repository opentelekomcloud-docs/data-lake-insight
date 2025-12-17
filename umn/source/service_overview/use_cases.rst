:original_name: dli_07_0002.html

.. _dli_07_0002:

Use Cases
=========

DLI is applicable to large-scale log analysis, federated analysis of heterogeneous data sources, and big data ETL processing.

Large-scale Log Analysis
------------------------

-  Gaming operations data analysis

   Different departments of a game company analyze daily new logs via the game data analysis platform to obtain required metrics and make decision based on the obtained metric data. For example, the operation department obtains required metric data, such as new players, active players, retention rate, churn rate, and payment rate, to learn the current game status and determine follow-up actions. The placement department obtains the channel sources of new players and active players to determine the platforms for placement in the next cycle.

-  Advantages

   -  Efficient Spark programming model: DLI directly ingests data from DIS and performs preprocessing such as data cleaning. You only need to edit the processing logic, without paying attention to the multi-thread model.
   -  Ease of use: You can use standard SQL statements to compile metric analysis logic without paying attention to the complex distributed computing platform.

Federated Analysis of Heterogeneous Data Sources
------------------------------------------------

-  Digital service transformation of automotive enterprises

   Facing new competition pressures in the market and continuous transformation of travel services, automotive enterprises build the IoV cloud platform and vehicle OS to streamline Internet applications and vehicle use cases and complete digital transformation for automotive enterprises. In this way, they can provide better smart travel experience for vehicle owners, improve their competitiveness, and promote sales growth. For example, DLI can be used to collect and analyze daily vehicle indicator data (such as battery, engine, tire pressure, and airbag health status) and provide maintenance suggestions to vehicle owners in a timely manner.

-  Advantages

   -  No need for migration in multi-source data analysis: RDS stores the basic information about vehicles and vehicle owners, table store saves real-time vehicle location and health status, and GaussDB(DWS) stores periodic metric statistics. DLI allows federated analysis on data from multiple sources without data migration.
   -  Tiered data storage: Car companies need to retain all historical data to support auditing and other services that require infrequent data access. Warm and cold data is stored in OBS and frequently accessed data is stored in GaussDB(DWS), reducing the overall storage cost.
   -  Rapid and agile alarm triggering: There are no special requirements for the CPU, memory, hard disk space, and bandwidth.

Big Data ETL Processing
-----------------------

-  Carrier big data analysis

   Carriers typically require petabytes, or even exabytes of data storage, for both structured (base station details) and unstructured (messages and communications) data. They need to be able to access the data with extremely low data latency. It is a major challenge to extract value from this data efficiently. DLI provides multi-mode engines such as batch processing and stream processing to break down data silos and perform unified data analysis.

-  Advantages

   -  Big data ETL: You can enjoy TB to EB-level data governance capabilities to quickly perform ETL processing on massive carrier data. Distributed datasets are provided for batch processing.
   -  High throughput and low latency: The Apache Flink dataflow model is used with high-performance compute resources to consume data from user-created Kafka, MRS-Kafka, and DMS-Kafka. A single CU can process 1,000 to 20,000 messages per second.
