:original_name: dli_03_0082.html

.. _dli_03_0082:

How Do I Run a Complex PySpark Program in DLI?
==============================================

DLI natively supports PySpark.

For most cases, Python is preferred for data analysis, and PySpark is the best choice for big data analysis. Generally, JVM programs are packed into JAR files and depend on third-party JAR files. Similarly, Python programs also depend on third-party libraries, especially big data analysis programs related to PySpark-based converged machine learning. Traditionally, Python libraries are installed directly on the execution machine using pip. However, for serverless services like DLI, users do not need to and are not aware of the underlying compute resources. How can we ensure that users can run their programs more effectively?

DLI has built-in algorithm libraries for machine learning in its compute resources. These common algorithm libraries meet the requirements of most users. What if a user's PySpark program depends on a program library that is not provided by the built-in algorithm library? Actually, the dependency of PySpark is specified based on PyFiles. On the DLI Spark job page, you can directly select the Python third-party program library (such as ZIP and EGG) stored on OBS.

The compressed package of the dependent third-party Python library has structure requirements. For example, if the PySpark program depends on moduleA (import moduleA), the compressed package must meet the following structure requirement:


.. figure:: /_static/images/en-us_image_0296823520.png
   :alt: **Figure 1** Compressed package structure requirement

   **Figure 1** Compressed package structure requirement

That is, the compressed package contains a folder named after a module name, and then the Python file of the corresponding class. Generally, the downloaded Python library may not meet this requirement. Therefore, you need to compress the Python library again. In addition, there is no requirement on the name of the compressed package. Therefore, it is recommended that you compress the packages of multiple modules into a compressed package. Now, a large and complex PySpark program is configured and runs normally.
