:original_name: dli_08_0110.html

.. _dli_08_0110:

Anomaly Detection
=================

Anomaly detection applies to various scenarios, including intrusion detection, financial fraud detection, sensor data monitoring, medical diagnosis, natural data detection, and more. The typical algorithms for anomaly detection include the statistical modeling method, distance-based calculation method, linear model, and nonlinear model.

DLI uses an anomaly detection method based on the random forest, which has the following characteristics:

-  The one-pass algorithm is used with O(1) amortized time complexity and O(1) space complexity.
-  The random forest structure is constructed only once. The model update operation only updates the node data distribution values.
-  The node stores data distribution information of multiple windows, and the algorithm can detect data distribution changes.
-  Anomaly detection and model updates are completed in the same code framework.

Syntax
------

::

   SRF_UNSUP(ARRAY[Field 1, Field 2, ...], 'Optional parameter list')

.. note::

   -  The anomaly score returned by the function is a DOUBLE value in the range of [0, 1].
   -  The field names must be of the same type. If the field types are different, you can use the CAST function to escape the field names, for example, [a, CAST(b as DOUBLE)].
   -  The syntax of the optional parameter list is as follows: "key1=value,key2=value2,..."

Parameter Description
---------------------

.. table:: **Table 1** Parameter Description

   +--------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | Parameter          | Mandatory       | Description                                                                                                                     | Default Value   |
   +====================+=================+=================================================================================================================================+=================+
   | transientThreshold | No              | Threshold for which the histogram change is indicating a change in the data.                                                    | 5               |
   +--------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | numTrees           | No              | Number of trees composing the random forest.                                                                                    | 15              |
   +--------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | maxLeafCount       | No              | Maximum number of leaf nodes one tree can have.                                                                                 | 15              |
   +--------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | maxTreeHeight      | No              | Maximum height of the tree.                                                                                                     | 12              |
   +--------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | seed               | No              | Random seed value used by the algorithm.                                                                                        | 4010            |
   +--------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | numClusters        | No              | Number of types of data to be detected. By default, the following two data types are available: anomalous and normal data.      | 2               |
   +--------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | dataViewMode       | No              | Algorithm learning mode.                                                                                                        | history         |
   |                    |                 |                                                                                                                                 |                 |
   |                    |                 | -  Value **history** indicates that all historical data is considered.                                                          |                 |
   |                    |                 | -  Value **horizon** indicates that only historical data of a recent time period (typically a size of 4 windows) is considered. |                 |
   +--------------------+-----------------+---------------------------------------------------------------------------------------------------------------------------------+-----------------+

Example
-------

Anomaly detection is conducted on the **c** field in data stream **MyTable**. If the anomaly score is greater than 0.8, then the detection result is considered to be anomaly.

::

   SELECT c,
       CASE WHEN SRF_UNSUP(ARRAY[c], "numTrees=15,seed=4010") OVER (ORDER BY proctime RANGE BETWEEN INTERVAL '99' SECOND PRECEDING AND CURRENT ROW) > 0.8
            THEN 'anomaly'
            ELSE 'not anomaly'
       END
   FROM MyTable
