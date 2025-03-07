.. Copyright (C) 2015–2022 Wazuh, Inc.

.. meta::
   :description: Learn how to use a preconfigured role to install the Wazuh indexer and customize the installation with different variables in this section.

Wazuh indexer
-------------

This role is intended to deploy the Wazuh indexer to a specified node. The following variables can be used to customize the installation:

-  ``indexer_network_hosts``: This defines the listening IP address (default: ``127.0.0.1``).
-  ``indexer_http_port``: This defines the listening port (default: ``9200``).
-  ``indexer_jvm_xms``: This specifies the amount of memory to be used for java (default: ``null``).

To use the role in a playbook, a YAML file ``wazuh-indexer.yml`` can be created with the contents below:

.. code-block:: yaml

   - hosts: indexer
     roles:
     - wazuh-indexer

Custom variable definitions for different environments can be set. For example:

-  For a production environment, the variables can be saved in ``vars-production.yml``:

   .. code-block:: yaml

      indexer_network_host: '10.1.1.10'

-  For a development environment, the variables can be saved in ``vars-development.yml``:

   .. code-block:: yaml

      indexer_network_host: '192.168.0.10'
        
To run the playbook for a specific environment, the command below is run:

.. code-block:: console

   $ ansible-playbook wazuh-indexer.yml -e@vars-production.yml

The example above will install the Wazuh Indexer and set the listening address to: ``10.1.1.10`` using ``vars-production.yml``.

Please review the :ref:`variables references <wazuh_ansible_reference_indexer>` section to see all variables available for this role.
