:original_name: dli_03_0107.html

.. _dli_03_0107:

How Do I Use Spark to Write Data into a DLI Table?
==================================================

To use Spark to write data into a DLI table, configure the following parameters:

-  fs.obs.access.key
-  fs.obs.secret.key
-  fs.obs.impl
-  fs.obs.endpoint

The following is an example:

.. code-block::

   import logging
   from operator import add
   from pyspark import SparkContext

   logging.basicConfig(format='%(message)s', level=logging.INFO)

   #import local file
   test_file_name = "D://test-data_1.txt"
   out_file_name = "D://test-data_result_1"

   sc = SparkContext("local","wordcount app")
   sc._jsc.hadoopConfiguration().set("fs.obs.access.key", "myak")
   sc._jsc.hadoopConfiguration().set("fs.obs.secret.key", "mysk")
   sc._jsc.hadoopConfiguration().set("fs.obs.impl", "org.apache.hadoop.fs.obs.OBSFileSystem")
   sc._jsc.hadoopConfiguration().set("fs.obs.endpoint", "myendpoint")

   # red: text_file rdd object
   text_file = sc.textFile(test_file_name)

   # counts
   counts = text_file.flatMap(lambda line: line.split(" ")).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a + b)
   # write
   counts.saveAsTextFile(out_file_name)
