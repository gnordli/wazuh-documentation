.. Copyright (C) 2022 Wazuh, Inc.

.. meta::
  :description: The Wazuh GCP module allows you to fetch logs from Google Pub/Sub and Google Storage. Learn how to configure the Wazuh GCP module in this section.

.. _gcp_considerations:

Considerations for configuration
================================

First execution
---------------

If no ``only_logs_after`` value was provided, the module will only fetch the logs of the date of the execution.

Older logs
----------

The ``Google Cloud Platform`` Wazuh module only looks for new logs in buckets based upon the key of the last processed log object, which includes the datetime stamp. If older logs are loaded or the ``only_logs_after`` option date is set to a datetime earlier than previous executions of the module, the older log files will be ignored and not ingested into Wazuh.


Creation time in Google Cloud Storage bucket contents
-----------------------------------------------------

When using the ``only_logs_after`` tag, the Wazuh module checks the creation time of each blob in the Google Cloud Storage bucket to determine if a file should be processed or not. This means that if the user manually moves any blob inside the specified bucket, its creation date changes and the ``gcp-module`` Wazuh module processes it again as it is considered a new blob.

Any date in the file's name is ignored and only the creation date is used to determine whether or not a file should be processed.

Logging level
-------------

To switch between different logging levels for debugging and troubleshooting purposes, the Google Cloud integration uses the :ref:`wazuh_modules.debug <wazuh_modules_options>` level to set its verbosity level.


Configuring multiple Google Cloud Storage bucket
------------------------------------------------

Below there is an example of a configuration that uses more than one bucket:

.. code-block:: xml

 <gcp-bucket>
    <run_on_start>yes</run_on_start>
    <interval>1m</interval>

    <bucket type="access_logs">
        <name>wazuh-test-bucket</name>
        <credentials_file>credentials.json</credentials_file>
    </bucket>

    <bucket type="access_logs">
        <name>wazuh-test-bucket-2</name>
        <credentials_file>credentials.json</credentials_file>
        <only_logs_after>2021-JUN-01</only_logs_after>
        <path>access_logs/</path>
    </bucket>

    <bucket type="access_logs">
        <name>wazuh-test-bucket-3</name>
        <credentials_file>credentials.json</credentials_file>
        <path>access_logs</path>
        <remove_from_bucket>no</remove_from_bucket>
    </bucket>

  </gcp-bucket>
