:original_name: dli_03_0106.html

.. _dli_03_0106:

Flink Job Performance Tuning
============================

Basic Concepts of Performance Tuning
------------------------------------

-  Data Stacking in a Consumer Group

   The accumulated data of a consumer group can be calculated by the following formula: Total amount of data to be consumed by the consumer group = Offset of the latest data - Offset of the data submitted to the consumer group

   If your Flink job is connected to the Kafka premium edition, you can view the customer group on the Cloud Eye console. To view consumer available messages, choose **Cloud Service Monitoring** > **Distributed Message Service** form the navigation pane. On the displayed page, select **Kafka Premium** and click the **Consumer Groups** tab. Click the Kafka instance name and select the target consumer group.

-  Back Pressure Status

   Back pressure status is working load status of an operator. The back pressure is determined by the ratio of threads blocked in the output buffer to the total taskManager threads. This ratio is calculated by periodically sampling of the taskManager thread stack. By default, if the ratio is less than 0.1, the back pressure status is OK. If the ratio ranges from 0.1 to 0.5, the backpressure status is LOW. If the ratio exceeds 0.5, the backpressure status is HIGH.

-  Delay

   Delay indicates the duration from the time when source data starts being processed to the time when data reaches the current operator. The data source periodically sends a LatencyMarker (current timestamp). After receiving the LatencyMarker, the downstream operator subtracts the timestamp from the current time to calculate the duration. You can view the back pressure status and delay of an operator on the Flink UI or in the task list of a job. Generally, high back pressure and delay occur in pairs.

Performance Analysis
--------------------

Due to Flink back pressure, the data source consumption rate can be lower than the production rate when performance of a Flink job is low. As a result, data is stacked in a Kafka consumer group. In this case, you can use back pressure and delay of the operator to find its performance bottleneck.

-  The following figure shows that the back pressure of the last operator (sink) of the job is normal (green), and the back pressure of the previous two operators is high (red).

   |image1|

   In this scenario, the performance bottleneck is the sink and the optimization is specific to the data source. For example, for the JDBC data source, you can adjust the write batch using **connector.write.flush.max-rows** and JDBC rewriting parameter **rewriteBatchedStatements=true** to optimize the performance.

-  The following figure shows a scenario where the back pressure of the last second operator is normal.

   |image2|

   In this scenario, the performance bottleneck is the Vertex2 operator. You can view the description about the function of the operator for further optimization.

-  The back pressure of all operators is normal, but data is stacked.

   |image3|

   In this scenario, the performance bottleneck is the source, and the performance is mainly affected by the data read speed. In this case, you can increase the number of Kafka partitions and the number of concurrent sources to solve the problem.

-  The following figure shows that the back pressure of an operator is high, and its subsequent concurrent operators do not have back pressure.

   |image4|

   In this scenario, the performance bottleneck is Vertex2 or Vertex3. To find out the specific bottleneck operator, enable inPoolUsage monitoring on the Flink UI page. If the inPoolUsage for operator concurrency is 100% for a long time, the corresponding operator is likely to be the performance bottleneck. In this case, you check the operator for further optimization.


   .. figure:: /_static/images/en-us_image_0000001161719815.png
      :alt: **Figure 1** inPoolUsage monitoring

      **Figure 1** inPoolUsage monitoring

Performance Tuning
------------------

-  Rocksdb state tuning

   Top N sorting, window aggregate calculation, and stream-stream join involve a large number of status operations. You can optimize the performance of state operations to improve the overall performance. You can try any of the following optimization methods:

   -  Increase the state operation memory and reduce the disk I/O.

      -  Increase the number of CU resources in a single slot.
      -  Set optimization parameters:

         -  taskmanager.memory.managed.fraction=xx
         -  state.backend.rocksdb.block.cache-size=xx
         -  state.backend.rocksdb.writebuffer.size=xx

   -  Enable the micro-batch mode to avoid frequent state operations.

      Set the following parameters:

      -  table.exec.mini-batch.enabled=true
      -  table.exec.mini-batch.allow-latency=xx
      -  table.exec.mini-batch.size=xx

   -  Use ultra-high I/O local disks to accelerate disk operations.

-  Group aggregation tuning

   The data skew problem is solved by Local-Global that divides a group aggregation into two stages: doing local aggregation in upstream first, and then global aggregation in downstream. To enable Local-global aggregation, set optimization parameter: **table.optimizer.aggphase-strategy=TWO_PHASE**

-  Tuning count distinct

   -  If the associated keys of count distinct are sparse, using Local-Globa cannot solve the problem of SPOF. In this case, you can configure the following parameters to optimize bucket splitting.

      -  table.optimizer.distinct-agg.split.enabled=true
      -  table.optimizer.distinct-agg.split.bucket-num=xx

   -  Replace CASE WHEN with FILTER:

      For example:

      .. code-block::

         COUNT(DISTINCT CASE WHEN flag IN ('android', 'iphone')THEN user_id ELSE NULL END) AS app_uv

      Can be changed to:

      .. code-block::

         COUNT(DISTINCT user_id) FILTER(WHERE flag IN ('android', 'iphone')) AS app_uv

-  Optimizing dimension table join

   The dimension table in joined with the key of each record in the left table. The matched in the cache is performed first. If no match is found, the remotely obtained data is used for matching. The optimization is as follows:

   -  Increase the JVM memory and the number of cached records.
   -  Set indexes for the dimension table to speed up query.

.. |image1| image:: /_static/images/en-us_image_0000001161715923.png
.. |image2| image:: /_static/images/en-us_image_0000001115396324.png
.. |image3| image:: /_static/images/en-us_image_0000001115399366.png
.. |image4| image:: /_static/images/en-us_image_0000001161799601.png
