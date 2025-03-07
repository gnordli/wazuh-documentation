.. Copyright (C) 2022 Wazuh, Inc.

.. meta::
  :description: Learn about the different ways to configure your AWS credentials when monitoring AWS services with Wazuh.
  
.. _amazon_credentials:

Configuring AWS credentials
===========================

In order to make the Wazuh AWS module pull log data from the different services, it will be necessary to provide access credentials so it can connect to them.

There are multiple ways to configure the AWS credentials:

- `Profiles`_
- `IAM Roles`_
- `IAM roles for EC2 instances`_
- `Environment variables`_
- `Insert the credentials into the configuration`_

Create an IAM User
------------------

Wazuh requires a user with permissions to pull log data from the different services. The easiest way to accomplish this is by creating a new IAM user in the AWS account.

1. Create a new user:

    Navigate to Services > IAM > Users

    .. thumbnail:: ../../../images/aws/aws-user.png
      :align: center
      :width: 70%

    Click on "Next: Permissions" to continue.

2. Confirm user creation and get credentials:

    .. thumbnail:: ../../../images/aws/aws-summary-user.png
      :align: center
      :width: 70%

Save the credentials, you will use them later to configure the module.

Depending on the service that will be monitored, the Wazuh user will need a different set of permissions. The permissions required for each service are explained on the page of each service listed in the :ref:`supported services <amazon_supported_services>` section.

Authenticating options
----------------------

Credentials can be loaded from different locations, you can either specify the credentials as they are in the previous block of configuration, assume an IAM role, or load them from other `Boto3 supported locations <http://boto3.readthedocs.io/en/latest/guide/configuration.html#configuring-credentials>`_.

.. _aws_profile:

Profiles
^^^^^^^^

You can define profiles in your credentials file (``~/.aws/credentials``) and specify those profiles on the bucket configuration.

.. note::
  A region must be also specified on the ``credentials`` file in order to make it work.

For example, the following credentials file defines three different profiles: *default*, *dev* and *prod*.

.. code-block:: ini

  [default]
  aws_access_key_id=foo
  aws_secret_access_key=bar
  region=us-east-1

  [dev]
  aws_access_key_id=foo2
  aws_secret_access_key=bar2
  region=us-east-1

  [prod]
  aws_access_key_id=foo3
  aws_secret_access_key=bar3
  region=us-east-1

To use the *prod* profile in the AWS integration you would use the following bucket configuration:

.. code-block:: xml

  <bucket type="cloudtrail">
    <name>my-bucket</name>
    <aws_profile>prod</aws_profile>
  </bucket>

IAM Roles
^^^^^^^^^

.. warning::
  This authentication method requires some credentials to be previously added to the configuration using any other authentication method.

IAM Roles can also be used to interact with the different AWS services. This section shows how to create a sample IAM role with read-only permissions to pull data from a bucket:

1. Go to Services > Security, Identity & Compliance > IAM.

    .. thumbnail:: ../../../images/aws/aws-create-role-1.png
      :align: center
      :width: 70%

2. Select Roles in the right menu and click on the **Create role** button:

    .. thumbnail:: ../../../images/aws/aws-create-role-2.png
      :align: center
      :width: 70%

3. Select S3 service and click on the **Next: Permissions** button:

    .. thumbnail:: ../../../images/aws/aws-create-role-4.png
      :align: center
      :width: 70%

4. Select the previously created policy:

    .. thumbnail:: ../../../images/aws/aws-create-role-5.png
      :align: center
      :width: 70%

5. Click on the **Create role** button:

    .. thumbnail:: ../../../images/aws/aws-create-role-6.png
      :align: center
      :width: 70%

6. Access to role summary and click on its policy name:

    .. thumbnail:: ../../../images/aws/aws-create-role-7.png
      :align: center
      :width: 70%

7. Add permissions so the new role can do *sts:AssumeRole* action:

    .. thumbnail:: ../../../images/aws/aws-create-role-8.png
      :align: center
      :width: 70%

8. Come back to the role summary, go to the *Trust relationships* tab and click on the **Edit trust relationship** button:

    .. thumbnail:: ../../../images/aws/aws-create-role-9.png
      :align: center
      :width: 70%

9. Add your user to the *Principal* tag and click on the **Update Trust Policy** button:

    .. thumbnail:: ../../../images/aws/aws-create-role-10.png
      :align: center
      :width: 70%

Once your role is created, just paste it on the bucket configuration:

.. code-block:: xml

  <bucket type="cloudtrail">
    <name>my-bucket</name>
    <access_key>xxxxxx</access_key>
    <secret_key>xxxxxx</secret_key>
    <iam_role_arn>arn:aws:iam::xxxxxxxxxxx:role/wazuh-role</iam_role_arn>
 </bucket>

IAM roles for EC2 instances
^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can use IAM roles and assign them to EC2 instances so there's no need to insert authentication parameters on the ``ossec.conf`` file. This is the recommended configuration. Find more information about IAM roles on EC2 instances in the official `Amazon AWS documentation <https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html>`_.

This is an example configuration:

.. code-block:: xml

  <bucket type="cloudtrail">
    <name>my-bucket</name>
  </bucket>

Environment variables
^^^^^^^^^^^^^^^^^^^^^

If you're using a single AWS account for all your buckets this could be the most suitable option for you. You just have to define the following environment variables:

* ``AWS_ACCESS_KEY_ID``
* ``AWS_SECRET_ACCESS_KEY``

Insert the credentials into the configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Another available option to set up credentials is writing them right into the Wazuh configuration file (``/var/ossec/etc/ossec.conf``), inside of the ``<bucket>`` block on the module configuration.

This is an example configuration:

.. code-block:: xml

  <bucket type="cloudtrail">
    <name>my-bucket</name>
    <access_key>insert_access_key</access_key>
    <secret_key>insert_secret_key</secret_key>
  </bucket>
