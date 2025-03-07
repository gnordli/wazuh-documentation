.. Copyright (C) 2022 Wazuh, Inc.

.. meta::
  :description: Learn about the wazuh-logcollector program that monitors configured files and commands for new log messages in this section of the documentation.

.. _wazuh-logcollector:

wazuh-logcollector
==================

.. versionadded:: 4.2

The wazuh-logcollector program monitors configured files and commands for new log messages.

``wazuh-logcollector`` is now multi-threaded, achieving an improvement in overall performance. Each of the threads will read the first log that is not already handled by other threads
and when it finishes reading, it will try to read the next available log (file or command) so that all the threads are always occupied.

In addition, the interlocking problem that existed in the one-threaded version when it took a long time to read a log while the rest were left unattended, is avoided.

.. warning:: The advantages of the multithreaded logcollector are only available from version 3.6.0 and higher.


+-----------------+-------------------------------------------------------------------------------------------------+
| **-c <config>** | Run using <config> as the configuration file.                                                   |
+                 +-------------------------------------------+-----------------------------------------------------+
|                 | Default value                             | /var/ossec/etc/ossec.conf                           |
+-----------------+-------------------------------------------+-----------------------------------------------------+
| **-d**          | Run in debug mode. This option may be repeated to increase the verbosity of the debug messages. |
+-----------------+-------------------------------------------------------------------------------------------------+
| **-f**          | Run in the foreground.                                                                          |
+-----------------+-------------------------------------------------------------------------------------------------+
| **-g <group>**  | Run as a specific group.                                                                        |
+-----------------+-------------------------------------------------------------------------------------------------+
| **-h**          | Display the help message.                                                                       |
+-----------------+-------------------------------------------------------------------------------------------------+
| **-t**          | Test configuration.                                                                             |
+-----------------+-------------------------------------------------------------------------------------------------+
| **-V**          | Display the version and license information                                                     |
+-----------------+-------------------------------------------------------------------------------------------------+
