.. Copyright (C) 2022 Wazuh, Inc.

.. meta::
  :description: Learn about the variables that facilitate the deployment of the Wazuh agent on Linux in this section of our documentation.

.. _deployment_variables_linux:

Deployment variables for Linux
==============================


For an agent to be fully deployed and connected to the Wazuh server, it needs to be installed, registered, and configured. The installers can use variables that allow configuration provisioning to make the process simple.

Below you can find a table describing the variables used by Wazuh installers and a few examples of how to use them.


+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                           | Description                                                                                                                                                                                          |
+==================================+======================================================================================================================================================================================================+
|   WAZUH_MANAGER                  |  Specifies the manager IP address or hostname. If you want to specify multiple managers, you can add them separated by commas. See :ref:`address <server_address>`.                                  |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_MANAGER_PORT             |  Specifies the manager connection port. See :ref:`port <server_port>`.                                                                                                                               |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_PROTOCOL                 |  Sets the communication protocol between the manager and the agent. Accepts UDP and TCP. The default is TCP. See :ref:`protocol <server_protocol>`.                                                  |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_SERVER      |  Specifies the Wazuh registration server, used for the agent registration. See :ref:`manager_address <enrollment_manager_address>`. If empty, the value set in ``WAZUH_MANAGER`` will be used.       |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_PORT        |  Specifies the port used by the Wazuh registration server. See :ref:`port <enrollment_manager_port>`.                                                                                                |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_PASSWORD    |  Sets password used to authenticate during register, stored in ``etc/authd.pass``. See :ref:`authorization_pass_path <enrollment_authorization_pass_path>`                                           |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_KEEP_ALIVE_INTERVAL      |  Sets the time between agent checks for manager connection. See :ref:`notify_time <notify_time>`.                                                                                                    |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_TIME_RECONNECT           |  Sets the time interval for the agent to reconnect with the Wazuh manager when connectivity is lost. See :ref:`time-reconnect  <time_reconnect>`.                                                    |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_CA          |  Host SSL validation need of Certificate of Authority. This option specifies the CA path. See :ref:`server_ca_path <enrollment_server_ca_path>`.                                                     |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_CERTIFICATE |  The SSL agent verification needs a CA signed certificate and the respective key. This option specifies the certificate path. See :ref:`agent_certificate_path <enrollment_agent_certificate_path>`. |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_KEY         |  Specifies the key path completing the required variables with WAZUH_REGISTRATION_CERTIFICATE for the SSL agent verification process. See :ref:`agent_key_path <enrollment_agent_key_path>`.         |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_AGENT_NAME               |  Designates the agent's name. By default, it will be the computer name. See :ref:`agent_name <enrollment_agent_name>`.                                                                               |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_AGENT_GROUP              |  Assigns the agent to one or more existing groups (separated by commas). See :ref:`agent_groups <enrollment_agent_groups>`.                                                                          |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   ENROLLMENT_DELAY               |  Assigns the time that agentd should wait after a successful registration. See :ref:`delay_after_enrollment <enrollment_delay_after_enrollment>`.                                                    |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples:

.. tabs::

     .. group-tab:: Yum


        * Registration with password:
   
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_PASSWORD="TopSecret" \
                  WAZUH_AGENT_NAME="yum-agent" yum install wazuh-agent
        
        * Registration with password and assigning a group:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_REGISTRATION_PASSWORD="TopSecret" \
                  WAZUH_AGENT_GROUP="my-group" yum install wazuh-agent
        
        * Registration with relative path to CA. It will be searched at your Wazuh installation folder:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_AGENT_NAME="yum-agent" \
                  WAZUH_REGISTRATION_CA="rootCA.pem" yum install wazuh-agent
        
        * Registration with protocol:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_AGENT_NAME="yum-agent" \
                  WAZUH_PROTOCOL="udp" yum install wazuh-agent
        
        * Registration and adding multiple address:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2,10.0.0.3" WAZUH_REGISTRATION_SERVER="10.0.0.2" \
                  WAZUH_AGENT_NAME="yum-agent" yum install wazuh-agent
        
        * Absolute paths to CA, certificate or key that contain spaces can be written as shown below:
        
        .. code-block:: console
        
             # WAZUH_MANAGER "10.0.0.2" WAZUH_REGISTRATION_SERVER "10.0.0.2" WAZUH_REGISTRATION_KEY "/var/ossec/etc/sslagent.key" \
                  WAZUH_REGISTRATION_CERTIFICATE "/var/ossec/etc/sslagent.cert" yum install wazuh-agent
        
        .. note:: It’s necessary to use both KEY and PEM options to verify agents' identities with the registration server. See the :ref:`Registration Service with host verification - Agent verification with host validation <enrollment_additional_security>` section.
     


     .. group-tab:: APT

        * Registration with password:
   
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_PASSWORD="TopSecret" \
                  WAZUH_AGENT_NAME="apt-agent" apt-get install wazuh-agent
        
        * Registration with password and assigning a group:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_REGISTRATION_PASSWORD="TopSecret" \
                  WAZUH_AGENT_GROUP="my-group" apt-get install wazuh-agent
        
        * Registration with relative path to CA. It will be searched at your Wazuh installation folder:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_AGENT_NAME="apt-agent" \
                  WAZUH_REGISTRATION_CA="rootCA.pem" apt-get install wazuh-agent
        
        * Registration with protocol:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_AGENT_NAME="apt-agent" \
                  WAZUH_PROTOCOL="udp" apt-get install wazuh-agent
        
        * Registration and adding multiple addresses:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2,10.0.0.3" WAZUH_REGISTRATION_SERVER="10.0.0.2" \
                  WAZUH_AGENT_NAME="apt-agent" apt-get install wazuh-agent
        
        * Absolute paths to CA, certificate or key that contain spaces can be written as shown below:
        
        .. code-block:: console
        
             # WAZUH_MANAGER "10.0.0.2" WAZUH_REGISTRATION_SERVER "10.0.0.2" WAZUH_REGISTRATION_KEY "/var/ossec/etc/sslagent.key" \
                  WAZUH_REGISTRATION_CERTIFICATE "/var/ossec/etc/sslagent.cert" apt-get install wazuh-agent
        
        .. note:: To verify agents identity with the registration server, it's necessary to use both KEY and PEM options. See the    :ref:`Registration Service with host verification - Agent verification with host validation <enrollment_additional_security>` section.
     
       



     .. group-tab:: ZYpp


        * Registration with password:
   
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_PASSWORD="TopSecret" \
                  WAZUH_AGENT_NAME="zypper-agent" zypper install wazuh-agent
        
        * Registration with password and assigning a group:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_REGISTRATION_PASSWORD="TopSecret" \
                  WAZUH_AGENT_GROUP="my-group" zypper install wazuh-agent
        
        * Registration with relative path to CA. It will be searched at your Wazuh installation folder:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_AGENT_NAME="zypper-agent" \
                  WAZUH_REGISTRATION_CA="rootCA.pem" zypper install wazuh-agent
        
        * Registration with protocol:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2" WAZUH_REGISTRATION_SERVER="10.0.0.2" WAZUH_AGENT_NAME="zypper-agent" \
                  WAZUH_PROTOCOL="udp" zypper install wazuh-agent
        
        * Registration and adding multiple address:
        
        .. code-block:: console
        
             # WAZUH_MANAGER="10.0.0.2,10.0.0.3" WAZUH_REGISTRATION_SERVER="10.0.0.2" \
                  WAZUH_AGENT_NAME="zypper-agent" zypper install wazuh-agent
        
        * Absolute paths to CA, certificate or key that contain spaces can be written as shown below:
        
        .. code-block:: console
        
             # WAZUH_MANAGER "10.0.0.2" WAZUH_REGISTRATION_SERVER "10.0.0.2" WAZUH_REGISTRATION_KEY "/var/ossec/etc/sslagent.key" \
                  WAZUH_REGISTRATION_CERTIFICATE "/var/ossec/etc/sslagent.cert" zypper install wazuh-agent
        
        .. note:: To verify agents identity with the registration server, it's necessary to use both KEY and PEM options. See the    :ref:`Registration Service with host verification - Agent verification with host validation <enrollment_additional_security>` section.



