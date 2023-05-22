:original_name: dli_01_0475.html

.. _dli_01_0475:

DLI Request Conditions
======================

Request conditions are useful in determining when a custom policy takes effect. A request condition consists of a condition key and operator. Condition keys are either global or service-level and are used in the Condition element of a policy statement. Global condition keys (starting with **g:**) are available for operations of all services, while service-level condition keys (starting with a service name such as **dli**) are available only for operations of a specific service. An operator is used together with a condition key to form a complete condition statement.

IAM provides a set of DLI predefined condition keys. The following table lists the predefined condition keys of *DLI*.

.. table:: **Table 1** DLI request conditions

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | Condition Key   | Type            | Operator        | Description                                                                                            |
   +=================+=================+=================+========================================================================================================+
   | g:CurrentTime   | Global          | Date and time   | Time when an authentication request is received                                                        |
   |                 |                 |                 |                                                                                                        |
   |                 |                 |                 | .. note::                                                                                              |
   |                 |                 |                 |                                                                                                        |
   |                 |                 |                 |    The time is expressed in the format defined by **ISO 8601**, for example, **2012-11-11T23:59:59Z**. |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:MFAPresent    | Global          | Boolean         | Whether multi-factor authentication is used during user login                                          |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:UserId        | Global          | String          | ID of the current login user                                                                           |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:UserName      | Global          | String          | Current login user                                                                                     |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:ProjectName   | Global          | String          | Project that you have logged in to                                                                     |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
   | g:DomainName    | Global          | String          | Domain that you have logged in to                                                                      |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------+
