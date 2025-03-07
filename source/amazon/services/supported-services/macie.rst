.. Copyright (C) 2022 Wazuh, Inc.

.. meta::
  :description: Amazon Macie is a service that uses machine learning and pattern matching to protect sensitive data. Learn how to configure and monitor it with Wazuh.

.. _amazon_macie:

Amazon Macie
============

`Amazon Macie <https://aws.amazon.com/macie/>`_ is a security service that uses machine learning to automatically discover, classify, and protect sensitive data in AWS. Macie recognizes sensitive data such as personally identifiable information (PII) or intellectual property and provides you with dashboards and alerts that give visibility into how this data is being accessed or moved. The fully managed service continuously monitors data access activity for anomalies and generates detailed alerts when it detects risk of unauthorized access or inadvertent data leaks.

Amazon configuration
--------------------

#. :doc:`Create a new </amazon/services/prerequisites/S3-bucket>` S3 bucket. (If you want to use an already created one, skip this step).

#. Go to Services > Analytics > Kinesis:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-4.png
      :align: center
      :width: 70%

    #. If it's the first time you're using this service, you'll see the following screen. Just click on *Get started*:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-4.1.png
      :align: center
      :width: 70%

#. Click on *Create delivery stream* button:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-5.png
      :align: center
      :width: 70%

#. Put a name to your delivery stream and click on the *Next* button at the bottom of the page:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-6.png
      :align: center
      :width: 70%

#. On the next page, leave both options as *Disabled* and click on *Next*:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-7.png
      :align: center
      :width: 70%

#. Select *Amazon S3* as the destination, then select the previously created S3 bucket and add a prefix where logs will be stored. AWS Firehose creates a file structure *YYYY/MM/DD/HH*, if a prefix is used the created file structure would be *firehose/YYYY/MM/DD/HH*. If a prefix is used it must be specified under the Wazuh Bucket configuration:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-8.png
      :align: center
      :width: 70%

#. You can select the compression you prefer. Wazuh supports any kind of compression but Snappy. After that, click on **Create new or choose**:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-9.png
      :align: center
      :width: 70%

#. Give a proper name to the role and click on the *Allow* button:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-10.png
      :align: center
      :width: 70%

#. The following page is just a summary of the Firehose stream created, go to the bottom of the page and click on the **Create delivery stream** button:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-11.png
      :align: center
      :width: 70%

#. Go to Services > Management Tools > CloudWatch:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-12.png
      :align: center
      :width: 70%

#. Select *Rules* on the left menu and click on the *Create rule* button:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-13.png
      :align: center
      :width: 70%

#. Select the services you want to get logs from using the **Service name** slider, then, click on the **Add target** button and add the previously created Firehose delivery stream there. Also, create a new role to access the delivery stream.

    .. thumbnail:: ../../../images/aws/aws-create-firehose-14.png
      :align: center
      :width: 70%

#. Give the rule some name and click on the *Create rule* button:

    .. thumbnail:: ../../../images/aws/aws-create-firehose-15.png
      :align: center
      :width: 70%

#. Once the rule is created, data will start to be sent to the previously created S3 bucket. Remember to first enable the service you want to monitor, otherwise, you won't get any data.

Policy configuration
^^^^^^^^^^^^^^^^^^^^

.. include:: /_templates/cloud/amazon/create_policy.rst
.. include:: /_templates/cloud/amazon/bucket_policies.rst
.. include:: /_templates/cloud/amazon/attach_policy.rst

Wazuh configuration
-------------------

#. Open the Wazuh configuration file (``/var/ossec/etc/ossec.conf``) and add the following block:

    .. code-block:: xml

      <wodle name="aws-s3">
        <disabled>no</disabled>
        <interval>10m</interval>
        <run_on_start>yes</run_on_start>
        <skip_on_error>yes</skip_on_error>
        <bucket type="custom">
          <name>wazuh-aws-wodle</name>
          <path>macie</path>
          <aws_profile>default</aws_profile>
        </bucket>
      </wodle>

    .. note::
      Check the :doc:`AWS S3 module </user-manual/reference/ossec-conf/wodle-s3>` reference manual to learn more about each setting.

#. Restart Wazuh in order to apply the changes:

    * If you're configuring a Wazuh manager:

      a. For Systemd:

      .. code-block:: console

        # systemctl restart wazuh-manager

      b. For SysV Init:

      .. code-block:: console

        # service wazuh-manager restart

    * If you're configuring a Wazuh agent:

      a. For Systemd:

      .. code-block:: console

        # systemctl restart wazuh-agent

      b. For SysV Init:

      .. code-block:: console

        # service wazuh-agent restart

Use cases
---------

Amazon S3 (Simple Storage Service) provides secure and reliable storage capacity in the cloud. When using this service, it is highly recommended to monitor it to detect data loss attacks.

Below are some use cases for Wazuh alerts built for S3.

Bucket removal
^^^^^^^^^^^^^^

Multiple alerts will be raised when a Bucket has been removed. Some examples are shown below:

.. thumbnail:: ../../../images/aws/aws-s3-1.png
  :align: center
  :width: 70%

.. thumbnail:: ../../../images/aws/aws-s3-1.png
  :align: center
  :width: 70%
