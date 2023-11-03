:original_name: dli_08_0358.html

.. _dli_08_0358:

userDefined Source Table
========================

Function
--------

You can call APIs to obtain data from the cloud ecosystem or an open source ecosystem and use the obtained data as input of Flink jobs.

Prerequisites
-------------

The customized source class needs to inherit the **RichParallelSourceFunction** class and specify the data type as Row.

For example, run **public class MySource extends RichParallelSourceFunction<Row>{}** to declare custom class **MySource**. You need to implement the **open**, **run**, **close**, and **cancel** functions. Encapsulate the class into a JAR file and upload the file through the UDF JAR on the SQL editing page.

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

Syntax
------

.. code-block::

   create table userDefinedSource (
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

   +---------------------------+-----------+-------------------------------------------------------------------------------------------------------+
   | Parameter                 | Mandatory | Description                                                                                           |
   +===========================+===========+=======================================================================================================+
   | connector.type            | Yes       | Source type. The value can only be **user-defined**, indicating a custom source.                      |
   +---------------------------+-----------+-------------------------------------------------------------------------------------------------------+
   | connector.class-name      | Yes       | Fully qualified class name of the source class                                                        |
   +---------------------------+-----------+-------------------------------------------------------------------------------------------------------+
   | connector.class-parameter | No        | Parameter of the constructor of the source class. Only one parameter of the string type is supported. |
   +---------------------------+-----------+-------------------------------------------------------------------------------------------------------+

Precautions
-----------

**connector.class-name** must be a fully qualified class name.

Example
-------

.. code-block::

   create table userDefinedSource (
     attr1 int,
     attr2 int
   )
   with (
     'connector.type' = 'user-defined',
     'connector.class-name' = 'xx.xx.MySource'
   );
