:original_name: dli_03_0044.html

.. _dli_03_0044:

Does a Flink JAR Job Support Configuration File Upload? How Do I Upload a Configuration File?
=============================================================================================

Configuration files can be uploaded for user-defined jobs (JAR).

#. Upload the configuration file to DLI through **Package Management**.
#. In the **Other Dependencies** area of the Flink JAR job, select the created DLI package.
#. Load the file through **ClassName.class.getClassLoader().getResource("userData/fileName")** in the code. In the file name, **fileName** indicates the name of the file to be accessed, and **ClassName** indicates the name of the class that needs to access the file.
