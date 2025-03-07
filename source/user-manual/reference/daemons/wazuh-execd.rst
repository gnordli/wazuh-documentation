.. Copyright (C) 2022 Wazuh, Inc.

.. meta::
  :description: Learn how the wazuh-execd program runs active responses by initiating the configured scripts in this section of the documentation.

.. _wazuh-execd:

wazuh-execd
===========

.. versionadded:: 4.2

The wazuh-execd program runs active responses by initiating the configured scripts. It also handles the socket needed to perform remote upgrades in the agents.

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
