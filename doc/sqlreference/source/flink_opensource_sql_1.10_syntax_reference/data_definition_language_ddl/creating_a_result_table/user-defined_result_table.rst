:original_name: dli_08_0347.html

.. _dli_08_0347:

User-defined Result Table
=========================

Function
--------

Write your Java code to insert the processed data into a specified database supported by your cloud service.

.. _dli_08_0347__en-us_topic_0000001163132424_en-us_topic_0000001177560413_section20477184319477:

Prerequisites
-------------

**Implement the custom sink class :**

The custom sink class is inherited from Flink open-source class **RichSinkFunction**. The data type is **Tuple2<Boolean, Row**>.

For example, define the **MySink** class by **public class MySink extends RichSinkFunction< Tuple2<Boolean, Row>>{}**, and implement the **open**, **invoke**, and **close** functions. A code example is as follows:

.. code-block::

   public class MySink extends RichSinkFunction<Tuple2<Boolean, Row>> {
       // Initialize the object.
       @Override
       public void open(Configuration parameters) throws Exception {}

       @Override
       // Implement the data processing logic.
       /* The in parameter contains two values. The first value is of the Boolean type. The value true indicates the insert or update operation, and the value false indicates the delete operation. If the interconnected sink does not support the delete operation, the deletion will not be executed. The second value indicates the data to be operated.*/
       public void invoke(Tuple2<Boolean, Row> in, Context context) throws Exception {}

       @Override
       public void close() throws Exception {}
   }

Content of the dependent pom configuration file is as follows:

.. code-block::

   <dependency>
       <groupId>org.apache.flink</groupId>
       <artifactId>flink-streaming-java_2.11</artifactId>
       <version>${flink.version}</version>
       <scope>provided</scope>
   </dependency>

   <dependency>
       <groupId>org.apache.flink</groupId>
       <artifactId>flink-core</artifactId>
       <version>${flink.version}</version>
       <scope>provided</scope>
   </dependency>

Pack the implemented class and compile it in a JAR file, and upload it using the UDF Jar parameter on the editing page of your Flink OpenSource SQL job.

Syntax
------

.. code-block::

   create table userDefinedSink (
     attr_name attr_type
     (',' attr_name attr_type)*
   )
   with (
     'connector.type' = 'user-defined',
     'connector.class-name' = ''
   );

Parameters
----------

.. table:: **Table 1** Parameter description

   +---------------------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                 | Mandatory | Description                                                                                                                                                                                                                    |
   +===========================+===========+================================================================================================================================================================================================================================+
   | connector.type            | Yes       | Connector type. The value can only be a user-defined sink.                                                                                                                                                                     |
   +---------------------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.class-name      | Yes       | Fully qualified class name of the sink class. For details about the implementation of the sink class, see :ref:`Prerequisites <dli_08_0347__en-us_topic_0000001163132424_en-us_topic_0000001177560413_section20477184319477>`. |
   +---------------------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.class-parameter | No        | Parameter of the constructor of the sink class. Only one parameter of the string type is supported.                                                                                                                            |
   +---------------------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

**connector.class-name** must be a fully qualified class name.

Example
-------

.. code-block::

   create table userDefinedSink (
     attr1 int,
     attr2 int
   )
   with (
     'connector.type' = 'user-defined',
     'connector.class-name' = 'xx.xx.MySink'
   );
