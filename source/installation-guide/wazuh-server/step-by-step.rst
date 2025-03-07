.. Copyright (C) 2015, Wazuh, Inc.

.. meta:: :description: The Wazuh server is in charge of analyzing the data received from the Wazuh agents. Install the Wazuh server in a single-node or multi-node configuration according to your environment needs.

Installing the Wazuh server step by step
========================================

Install and configure the Wazuh server as a single-node or multi-node cluster following step-by-step instructions. The Wazuh server is a central component that includes the Wazuh manager and Filebeat. The Wazuh manager collects and analyzes data from the deployed Wazuh agents. It triggers alerts when threats or anomalies are detected. Filebeat securely forwards alerts and archived events to the Wazuh indexer.

The installation process is divided into two stages.  

#. Wazuh server node installation

#. Cluster configuration for multi-node deployment 

.. note:: Root user privileges are required to run the commands described below.

1. Wazuh server node installation
----------------------------------
.. raw:: html

  <div class="accordion-section open">

Adding the Wazuh repository
^^^^^^^^^^^^^^^^^^^^^^^^^^^

  .. note::
    If you are installing the Wazuh server on the same host as the Wazuh indexer, you may skip these steps as you may have added the Wazuh repository already.

  ..
    Add the Wazuh repository to download the official Wazuh packages. As an alternative, you can download the Wazuh packages directly from :doc:`../packages-list`.
      
  .. tabs::


    .. group-tab:: Yum


      .. include:: /_templates/installations/common/yum/add-repository.rst



    .. group-tab:: APT


      .. include:: /_templates/installations/common/deb/add-repository.rst




Installing the Wazuh manager
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  #. Install the Wazuh manager package. 

     .. tabs::
     
     
       .. group-tab:: Yum
     
     
         .. include:: /_templates/installations/wazuh/yum/install_wazuh_manager.rst
     
     
     
       .. group-tab:: APT
     
     
         .. include:: /_templates/installations/wazuh/deb/install_wazuh_manager.rst
     
     
     
  #. Enable and start the Wazuh manager service.

      .. include:: /_templates/installations/wazuh/common/enable_wazuh_manager_service.rst


  #. Run the following command to verify the Wazuh manager status. 

      .. include:: /_templates/installations/wazuh/common/check_wazuh_manager.rst



.. _wazuh_server_multi_node_filebeat:

Installing Filebeat
^^^^^^^^^^^^^^^^^^^

  #. Install the Filebeat package.

      .. tabs::


        .. group-tab:: Yum


          .. include:: /_templates/installations/filebeat/common/yum/install_filebeat.rst



        .. group-tab:: APT


          .. include:: /_templates/installations/filebeat/common/apt/install_filebeat.rst



Configuring Filebeat 
^^^^^^^^^^^^^^^^^^^^

  #. Download the preconfigured Filebeat configuration file.

      .. code-block:: console

        # curl -so /etc/filebeat/filebeat.yml https://packages.wazuh.com/|WAZUH_CURRENT_MINOR|/tpl/wazuh/filebeat/filebeat.yml
        
        
  #. Edit the ``/etc/filebeat/filebeat.yml`` configuration file and replace the following value:

     .. include:: /_templates/installations/filebeat/opensearch/configure_filebeat.rst

  #. Create a Filebeat keystore to securely store authentication credentials.

      .. code-block:: console
     
        # filebeat keystore create

  #. Add the username and password ``admin``:``admin`` to the secrets keystore.
      
      .. code-block:: console

        # echo admin | filebeat keystore add username --stdin --force
        # echo admin | filebeat keystore add password --stdin --force    

  #. Download the alerts template for the Wazuh indexer.

      .. include:: /_templates/installations/filebeat/opensearch/load_filebeat_template.rst


  #. Install the Wazuh module for Filebeat.

      .. code-block:: console

        # curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.2.tar.gz | tar -xvz -C /usr/share/filebeat/module

Deploying certificates
^^^^^^^^^^^^^^^^^^^^^^

  .. note::
    Make sure that a copy of ``wazuh-certificates.tar``, created during the Wazuh indexer installation, is placed in your working directory.

  #. Replace ``<server-node-name>`` with your Wazuh server node certificate name, the same used in ``config.yml`` when creating the certificates. Then, move the certificates to their corresponding location.

      .. include:: /_templates/installations/filebeat/opensearch/copy_certificates_filebeat_wazuh_cluster.rst

      
Starting the Filebeat service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  #. Enable and start the Filebeat service.

      .. include:: /_templates/installations/filebeat/common/enable_filebeat.rst

  #. Run the following command to verify that Filebeat is successfully installed.

     .. code-block:: console

        # filebeat test output

     Expand the output to see an example response.
     
     .. code-block:: none
          :class: output accordion-output
     
          elasticsearch: https://127.0.0.1:9200...
            parse url... OK
            connection...
              parse host... OK
              dns lookup... OK
              addresses: 127.0.0.1
              dial up... OK
            TLS...
              security: server's certificate chain verification is enabled
              handshake... OK
              TLS version: TLSv1.3
              dial up... OK
            talk to server... OK
            version: 7.10.2


Your Wazuh server node is now successfully installed. Repeat the steps of this installation process stage for every Wazuh server node in your cluster and carry on then with configuring the Wazuh cluster. If you want a Wazuh server single-node cluster, everything is set and you can proceed directly with :doc:`../wazuh-dashboard/step-by-step`.
  
2. Cluster configuration for multi-node deployment
--------------------------------------------------
.. raw:: html

  <div class="accordion-section">

After completing the installation of the Wazuh server on every node, you need to configure one server node only as the master and the rest as workers.

.. _wazuh_server_master_node:

Configuring the Wazuh server master node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  #. Edit the following settings in the ``/var/ossec/etc/ossec.conf`` configuration file.

      .. include:: /_templates/installations/manager/configure_wazuh_master_node.rst

  #. Restart the Wazuh manager. 

      .. include:: /_templates/installations/manager/restart_wazuh_manager.rst

.. _wazuh_server_worker_nodes:
    
Configuring the Wazuh server worker nodes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  #. .. include:: /_templates/installations/manager/configure_wazuh_worker_node.rst

  #. Restart the Wazuh manager. 

      .. include:: /_templates/installations/manager/restart_wazuh_manager.rst

  Repeat these configuration steps for every Wazuh server worker node in your cluster.

Testing Wazuh server cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  .. include:: /_templates/installations/manager/check_wazuh_cluster.rst

Next steps
----------

The Wazuh server installation is now complete and you can proceed with :doc:`../wazuh-dashboard/step-by-step`.

If you want to uninstall the Wazuh server, see :ref:`uninstall_server`.
