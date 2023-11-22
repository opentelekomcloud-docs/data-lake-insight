:original_name: dli_03_0102.html

.. _dli_03_0102:

How Do I View the Resource Usage of DLI Spark Jobs?
===================================================

Viewing the Configuration of a Spark Job
----------------------------------------

Log in to the DLI console. In the navigation pane, choose **Job Management** > **Spark Jobs**. In the job list, locate the target job and click |image1| next to Job ID to view the parameters of the job.

.. note::

   The content is displayed only when the parameters in **Advanced Settings** are configured during Spark job creation.

Viewing Real-Time Resource Usage of a Spark Job
-----------------------------------------------

Perform the following operations to view the number of running CUs occupied by a Spark job in real time:

#. Log in to the DLI console. In the navigation pane, choose **Job Management** > **Spark Jobs**. In the job list, locate the target job and click **SparkUI** in the **Operation** column.

#. On the Spark UI page, view the real-time running resources of the Spark job.


   .. figure:: /_static/images/en-us_image_0000001103931700.png
      :alt: **Figure 1** SparkUI

      **Figure 1** SparkUI

#. On the Spark UI page, view the original configuration of the Spark job (available only to new clusters).

   On the Spark UI page, click **Environment** to view **Driver** and **Executor** information.


   .. figure:: /_static/images/en-us_image_0000001150931533.png
      :alt: **Figure 2** Driver information

      **Figure 2** Driver information


   .. figure:: /_static/images/en-us_image_0000001104091730.png
      :alt: **Figure 3** Executor information

      **Figure 3** Executor information

.. |image1| image:: /_static/images/en-us_image_0000001103772632.png
