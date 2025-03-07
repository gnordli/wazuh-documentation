.. Copyright (C) 2022 Wazuh, Inc.

.. _setup_puppet:

Set up Puppet
=============

In this section, we are going to give a short explanation of how to install the different instances of Puppet. For a more detailed guide, go to the `official Puppet documentation. <https://puppet.com/docs/puppet/latest/puppet_index.html>`_

Before we get started with Puppet, confirm that the following network requirements are met:

- **Private network DNS**: Forward and reverse DNS must be configured, and every server must have a unique hostname. If you do not have DNS configured, you must use your hosts file for name resolution. We will assume that you will use your private network for communication within your infrastructure.
- **Firewall open ports**: The Puppet master must be reachable on TCP port 8140.

.. note::
    This guide has been made using Puppet version 7.16. Root user privileges are required to execute all the commands described below.

.. topic:: Contents

    .. toctree::
        :maxdepth: 1

        install-puppet-master.rst
        install-puppet-agent.rst        
        setup-puppet-certificates.rst
