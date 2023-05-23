:original_name: dli_03_0118.html

.. _dli_03_0118:

How Do I Read Uploaded Files for a Spark Jar Job?
=================================================

You can use SparkFiles to read the file submitted using **--file** form a local path: **SparkFiles.get(**\ *"Name of the uploaded file"*\ **)**.

.. note::

   -  The file path in the Driver is different from that obtained by the Executor. The path obtained by the Driver cannot be passed to the Executor.
   -  You still need to call **SparkFiles.get(**\ *"filename"*\ **)** in Executor to obtain the file path.
   -  The **SparkFiles.get()** method can be called only after Spark is initialized.

The java code is as follows:

.. code-block::

   package main.java

   import org.apache.spark.SparkFiles
   import org.apache.spark.sql.SparkSession

   import scala.io.Source

   object DliTest {
     def main(args:Array[String]): Unit = {
       val spark = SparkSession.builder
         .appName("SparkTest")
         .getOrCreate()

       // Driver: obtains the uploaded file.
       println(SparkFiles.get("test"))

       spark.sparkContext.parallelize(Array(1,2,3,4))
            // Executor: obtains the uploaded file.
         .map(_ => println(SparkFiles.get("test")))
         .map(_ => println(Source.fromFile(SparkFiles.get("test")).mkString)).collect()
     }
   }
