.. Copyright (C) 2022 Wazuh, Inc.

.. _upgrading_elastic_stack_2.x_5.x:

Upgrading Elastic Stack from 2.x to 5.x
=======================================

Although Wazuh 2.x is compatible with both Elastic Stack 2.x and 5.x, it is recommended to install version 5.x since the Wazuh Kibana plugin is not compatible with Elastic Stack 2.x.

Choose one from the following scenarios:

- `Continue using Elastic Stack 2.x`_

- `Upgrading from Elastic Stack 2.x to 5.x`_

Continue using Elastic Stack 2.x
--------------------------------

In this scenario, the user has to configure Logstash to receive data from Filebeat or directly read alerts generated by the Wazuh manager if a single-host architecture is used, feeding Elasticsearch using the Wazuh alerts template:

Configuring Logstash
^^^^^^^^^^^^^^^^^^^^

#. Download the new Logstash configuration:

    .. code-block:: console

      # curl -so /etc/logstash/conf.d/01-wazuh.conf https://raw.githubusercontent.com/wazuh/wazuh/2.1/extensions/logstash/01-wazuh.conf
      # curl -so /etc/logstash/wazuh-elastic2-template.json https://raw.githubusercontent.com/wazuh/wazuh/2.1/extensions/elasticsearch/wazuh-elastic2-template.json

#. In the output section of the ``/etc/logstash/conf.d/01-wazuh.conf`` file, comment the line containing ``elastic5-template`` and uncomment the line containing ``elastic2-template``:

    .. code-block:: pkgconfig

      output {
        elasticsearch {
        hosts => ["localhost:9200"]
        index => "wazuh-alerts-%{+YYYY.MM.dd}"
        document_type => "wazuh"
              #      template => "/etc/logstash/wazuh-elastic5-template.json"
  	          template => "/etc/logstash/wazuh-elastic2-template.json"
  	          template_name => "wazuh"
  	          template_overwrite => true
  	    }
      }

#. If a ``single-host architecture`` is used, where the Wazuh server is installed along with Elastic Stack on the same host, edit the ``/etc/logstash/conf.d/01-wazuh.conf`` file commenting the entire input section titled ``Remote Wazuh server - Filebeat input`` and uncommenting the entire input section titled ``Local Wazuh server - JSON file input``:

    .. code-block:: pkgconfig

      # Wazuh - Logstash configuration file
      ## Remote Wazuh server - Filebeat input
      #input {
      #beats {
      #      port => 5000
      #      codec => "json_lines"
      #      ssl => true
      #      ssl_certificate => "/etc/logstash/logstash.crt"
      #      ssl_key => "/etc/logstash/logstash.key"
      #  }
      #}
      # Local Wazuh server - JSON file input
      input {
          file {
              type => "wazuh-alerts"
              path => "/var/ossec/logs/alerts/alerts.json"
              codec => "json"
          }
      }
      ...

The configuration above will set up Logstash to read the Wazuh ``alerts.json`` file directly from the local filesystem rather than receive forwarded data from Filebeat.

Configuring Kibana
^^^^^^^^^^^^^^^^^^

In order to display the Wazuh alerts data, configure the Kibana index pattern:

#. Go to the *Settings* tab and configure a new wildcard:

    .. thumbnail:: ../../../images/installation/kibana-elk-settings.png
      :align: center
      :width: 100%

#. Set the ``wazuh-*`` as an index pattern and choose the ``timestamp`` as a time field. Then, click on the ``create`` button:

    .. thumbnail:: ../../../images/installation/kibana-elk-2.png
      :align: center
      :width: 100%

#. Set this wildcard as default by clicking on the ``star icon``:

    .. thumbnail:: ../../../images/installation/kibana-elk.png
      :align: center
      :width: 100%

#. Go to the *Discover* tab in order to visualize the alerts data.

Upgrading from Elastic Stack 2.x to 5.x
---------------------------------------

Follow these steps to upgrade Elastic Stack to version 5.x:

#. Stop the Logstash, Elasticsearch, and Kibana services:

    .. tabs::

      .. group-tab:: Systemd

        .. code-block:: console

            # systemctl stop logstash
            # systemctl stop elasticsearch
            # systemctl stop kibana

      .. group-tab:: SysV Init

        .. code-block:: console

          # service logstash stop
          # service elasticsearch stop
          # service kibana stop

#. Remove the old Logstash configuration and template files:

    .. tabs::

      .. group-tab:: Single-host architecture

        This step has to be done if the Wazuh server and Elastic Stack are installed on the same system:

        .. code-block:: console

         # rm /etc/logstash/conf.d/01-ossec-singlehost.conf
         # rm /etc/logstash/elastic-ossec-template.json

      .. group-tab:: Multitier server

        This step has to be done if Elastic Stack is installed on a standalone system:

        .. code-block:: console

         # rm /etc/logstash/conf.d/01-ossec.conf
         # rm /etc/logstash/elastic-ossec-template.json

#. Remove deprecated settings from the Elasticsearch configuration files:

    Removing deprecated settings on Elasticsearch will avoid errors and conflicts after the upgrade. To do this, comment the following lines in the ``/etc/elasticsearch/elasticsearch.yml`` configuration file:

      .. code-block:: yaml

        index.number_of_shards: 1
        index.number_of_replicas: 0

      The ``ES_HEAP_SIZE`` option is now deprecated and should be removed or commented in the ``/etc/sysconfig/elasticsearch`` file:

      .. code-block:: yaml

        # ES_HEAP_SIZE - Set it to half your system RAM memory
        ES_HEAP_SIZE=8g

    The next step consists in configuring Elasticsearch following the Elastic `jvm.options <https://www.elastic.co/guide/en/elasticsearch/reference/master/heap-size.html>`_ guide.

#. Install the newest version of Elastic Stack 5.x. Follow the appropriate link below for installation instructions for the desired operating system:

    - `Install Elastic Stack with RPM packages <https://documentation.wazuh.com/2.1/installation-guide/installing-elastic-stack/elastic_server_rpm.html#elastic-server-rpm>`_
    - `Install Elastic Stack with DEB packages <https://documentation.wazuh.com/2.1/installation-guide/installing-elastic-stack/elastic_server_deb.html#elastic-server-deb>`_

#. Check the software version of the Elasticsearch components to verify that the update was successful:

  a) For Logstash:

    .. code-block:: console

      # /usr/share/logstash/bin/logstash -V

    .. code-block:: none
      :class: output

      logstash 5.2.2

  b) For Elasticsearch:

    .. code-block:: console

      # /usr/share/elasticsearch/bin/elasticsearch -V

    .. code-block:: none
      :class: output

      Version: 5.2.2, Build: f9d9b74/2017-02-24T17:26:45.835Z, JVM: 1.8.0_60

  c) For Kibana:

    .. code-block:: console

      # /usr/share/kibana/bin/kibana -V

    .. code-block:: none
      :class: output

      5.2.

.. note:: Wazuh 2.x uses different indices and templates than Wazuh 1.x. After the upgrade, the previous alerts will not be seen in Kibana. In order to access these alerts, the previous indices have to be reindexed.
