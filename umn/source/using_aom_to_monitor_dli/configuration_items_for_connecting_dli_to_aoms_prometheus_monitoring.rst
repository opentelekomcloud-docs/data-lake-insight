:original_name: dli_01_0666.html

.. _dli_01_0666:

Configuration Items for Connecting DLI to AOM's Prometheus Monitoring
=====================================================================

When connecting DLI to AOM's Prometheus Monitoring, the system automatically configures the parameters listed in table 1 in :ref:`Configuration Items for Connecting DLI to AOM's Prometheus Monitoring <dli_01_0666>`. If the default configurations do not meet your requirements, you can manually configure the following parameters on the **Runtime Configuration** tab on the right of the Flink job's editing page. Your configurations are preferred.

.. table:: **Table 1** Configuration items for Connecting DLI to AOM Prometheus Monitoring

   +--------------------------------------------+-----------+---------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                  | Mandatory | Default Value | Data Type | Default Value | Description                                                                                                                                 |
   +============================================+===========+===============+===========+===============+=============================================================================================================================================+
   | metrics.reporter.remote.interval           | Yes       | None          | Duration  | 30 SECONDS    | Metric reporting period, which is recommended to be 30 seconds. If you set it to a smaller value, the metric is frequently reported.        |
   +--------------------------------------------+-----------+---------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | metrics.reporter.remote.remote-write-url   | Yes       | None          | String    | ``-``         | Remote write URL of AOM Prometheus common instances.                                                                                        |
   |                                            |           |               |           |               |                                                                                                                                             |
   |                                            |           |               |           |               | You do not need to manually add **https://** to the URL when configuring it. The system will automatically include **https://** in the URL. |
   |                                            |           |               |           |               |                                                                                                                                             |
   |                                            |           |               |           |               | Example configuration:                                                                                                                      |
   |                                            |           |               |           |               |                                                                                                                                             |
   |                                            |           |               |           |               | .. code-block::                                                                                                                             |
   |                                            |           |               |           |               |                                                                                                                                             |
   |                                            |           |               |           |               |    aom-internal-access.{regionId}.xxxxx.com:8xx3/v1/{projectId}/{prometheusId}/push                                                         |
   +--------------------------------------------+-----------+---------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | metrics.reporter.remote.report-all-metrics | No        | false         | Boolean   | false         | Whether all metrics are reported. The default value is **false**, indicating that only basic metrics are reported.                          |
   +--------------------------------------------+-----------+---------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | metrics.reporter.remote.pool-name          | No        | None          | String    | ``-``         | Add the name of the elastic resource pool where the current job is located as a tag to the metric.                                          |
   +--------------------------------------------+-----------+---------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | metrics.reporter.remote.dli-job-id         | No        | None          | String    | ``-``         | Add the ID of the current DLI Flink job as a tag to the metric.                                                                             |
   +--------------------------------------------+-----------+---------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | metrics.reporter.remote.dli-job-name       | No        | None          | String    | ``-``         | Add the name of the current DLI Flink job as a tag to the metric.                                                                           |
   +--------------------------------------------+-----------+---------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------+
