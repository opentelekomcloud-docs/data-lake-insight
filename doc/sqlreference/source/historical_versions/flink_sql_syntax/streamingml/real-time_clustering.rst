:original_name: dli_08_0216.html

.. _dli_08_0216:

Real-Time Clustering
====================

Clustering algorithms belong to unsupervised algorithms. K-Means, a clustering algorithm, partitions data points into related clusters by calculating the distance between data points based on the predefined cluster quantity. For offline static datasets, we can determine the clusters based on field knowledge and run K-Means to achieve a better clustering effect. However, online real-time streaming data is always changing and evolving, and the cluster quantity is likely to change. To address clustering issues on online real-time streaming data, DLI provides a low-delay online clustering algorithm that does not require predefined cluster quantity.

The algorithm works as follows: Given a distance function, if the distance between two data points is less than a threshold, both data points will be partitioned into the same cluster. If the distances between a data point and the central data points in several cluster centers are less than the threshold, then related clusters will be merged. When data in a data stream arrives, the algorithm computes the distances between each data point and the central data points of all clusters to determine whether the data point can be partitioned into to an existing or new cluster.

Syntax
------

::

   CENTROID(ARRAY[field_names], distance_threshold): Compute the centroid of the cluster where the current data point is assigned.
   CLUSTER_CENTROIDS(ARRAY[field_names], distance_threshold): Compute all centroids after the data point is assigned.
   ALL_POINTS_OF_CLUSTER(ARRAY[field_names], distance_threshold): Compute all data points in the cluster where the current data point is assigned.
   ALL_CLUSTERS_POINTS(ARRAY[field_names], distance_threshold): Computers all data points in each cluster after the current data point is assigned.

.. note::

   -  Clustering algorithms can be applied in **unbounded streams**.

Parameters
----------

.. table:: **Table 1** Parameters

   +--------------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory | Description                                                                                                                                   |
   +====================+===========+===============================================================================================================================================+
   | field_names        | Yes       | Name of the field where the data is located in the data stream. Multiple fields are separated by commas (,). For example, **ARRAY[a, b, c]**. |
   +--------------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | distance_threshold | Yes       | Distance threshold. When the distance between two data points is less than the threshold, both data points are placed in the same cluster.    |
   +--------------------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Use four functions to compute information related to clusters over windows.

::

   SELECT
     CENTROID(ARRAY[c,e], 1.0) OVER (ORDER BY proctime RANGE UNBOUNDED PRECEDING) AS centroid,
     CLUSTER_CENTROIDS(ARRAY[c,e], 1.0) OVER (ORDER BY proctime RANGE UNBOUNDED PRECEDING) AS centroids
   FROM MyTable

   SELECT
     CENTROID(ARRAY[c,e], 1.0) OVER (ORDER BY proctime RANGE BETWEEN INTERVAL '60' MINUTE PRECEDING AND CURRENT ROW) AS centroidCE,
     ALL_POINTS_OF_CLUSTER(ARRAY[c,e], 1.0) OVER (ORDER BY proctime RANGE BETWEEN INTERVAL '60' MINUTE PRECEDING AND CURRENT ROW) AS itemList,
     ALL_CLUSTERS_POINTS(ARRAY[c,e], 1.0) OVER (ORDER BY proctime RANGE  BETWEEN INTERVAL '60' MINUTE PRECEDING AND CURRENT ROW) AS listoflistofpoints
   FROM MyTable
