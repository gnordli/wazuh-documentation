.. Copyright (C) 2022 Wazuh, Inc.

.. meta::
  :description: The pre-built Wazuh Virtual Machine includes all Wazuh components ready-to-use. Test all Wazuh capabilities with our OVA.  

.. _virtual_machine:

Virtual Machine (OVA)
=====================

Wazuh provides a pre-built virtual machine image in Open Virtual Appliance (OVA) format. This can be directly imported to VirtualBox or other OVA compatible virtualization systems. Take into account that this VM only runs on 64-bit systems. It does not provide high availability and scalability out of the box. However, these can be implemented by using :doc:`distributed deployment </installation-guide/index>`.


Download the `virtual appliance (OVA) <https://packages.wazuh.com/|WAZUH_CURRENT_MAJOR_OVA|/vm/wazuh-|WAZUH_CURRENT_OVA|.ova>`_, which contains the following components:

    - CentOS 7
    - Wazuh manager |WAZUH_CURRENT_OVA|
    - Wazuh indexer |WAZUH_CURRENT_OVA|
    - Filebeat-OSS |FILEBEAT_LATEST_OVA|
    - Wazuh dashboard |WAZUH_CURRENT_OVA|


Hardware requirements
---------------------

The following requirements have to be in place before the Wazuh VM can be imported into a host operating system:

- The host operating system has to be a 64-bit system. 
- Hardware virtualization has to be enabled on the firmware of the host.
- A virtualization platform, such as VirtualBox, should be installed on the host system.

Out of the box, the Wazuh VM is configured with the following specifications:

.. |OVA_COMPONENT| replace:: Wazuh v|WAZUH_CURRENT_OVA| OVA

+------------------+----------------+--------------+--------------+
|    Component     |   CPU (cores)  |   RAM (GB)   | Storage (GB) |
+==================+================+==============+==============+
| |OVA_COMPONENT|  |       4        |      8       |     50       |
+------------------+----------------+--------------+--------------+

However, this hardware configuration can be modified depending on the number of protected endpoints and indexed alert data. More information about requirements can be found :doc:`here </quickstart>`. 


Import and access the virtual machine
-------------------------------------

First, import the OVA to the virtualization platform and start the machine.

Use the following user and password to access the virtual machine. You can use the virtualization platform or access it via SSH.
 
  .. code-block:: none

      user: wazuh-user
      password: wazuh

The password for ``root`` user is ``wazuh``. However, accessing the virtual machine via SSH is only possible using the system user. SSH login with the root user is disabled.

Access the Wazuh dashboard
--------------------------

Shortly after starting the VM, the Wazuh dashboard can be accessed from the web interface by using the following credentials:

  .. code-block:: none

     URL: https://<wazuh_server_ip>
     user: admin
     password: admin


You can find ``<wazuh_server_ip>``  by typing the following command in the VM:

  .. code-block:: none

     ip a


Configuration files
-------------------

All components included in this virtual image are configured to work out-of-the-box, without the need to modify any settings. However, all components can be fully customized. These are the configuration files locations:

  - Wazuh manager: ``/var/ossec/etc/ossec.conf``

  - Wazuh indexer: ``/etc/wazuh-indexer/opensearch.yml``
  
  - Filebeat-OSS: ``/etc/filebeat/filebeat.yml``
  
  - Wazuh dashboard: 

     - ``/etc/wazuh-dashboard/opensearch_dashboards.yml``

     - ``/usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml``

VirtualBox time configuration
-----------------------------

In case of using VirtualBox, once the virtual machine is imported it may run into issues caused by time skew when VirtualBox synchronizes the time of the guest machine. To avoid this situation, enable the ``Hardware Clock in UTC Time`` option in the ``System`` tab of the virtual machine configuration.

.. note::
  By default, the network interface type is set to Bridged Adapter. The VM will attempt to obtain an IP address from the network DHCP server. Alternatively, a static IP address can be set by configuring the appropriate network files in the CentOS operating system on which the VM is based.


Once the virtual machine is imported and running, the next step is to :doc:`deploy the Wazuh agents </installation-guide/wazuh-agent/index>` on the systems to be monitored.


Upgrading the VM
----------------

The virtual machine can be upgraded as a traditional installation:

  - :ref:`Upgrading the Wazuh manager <upgrading_wazuh_server>`
