.. Copyright (C) 2022 Wazuh, Inc.

.. meta::
  :description: Wazuh 3.3.1 has been released. Check out our release notes to discover the changes and additions of this release.
.. _release_3_3_1:

3.3.1 Release notes - 18 June 2018
==================================

This section shows the most relevant improvements and fixes in version 3.3.1. More details about these changes are provided in each component changelog.

- `wazuh/wazuh <https://github.com/wazuh/wazuh/blob/v3.3.1/CHANGELOG.md>`_
- `wazuh/wazuh-api <https://github.com/wazuh/wazuh-api/blob/v3.3.1/CHANGELOG.md>`_
- `wazuh/wazuh-ruleset <https://github.com/wazuh/wazuh-ruleset/blob/v3.3.1/CHANGELOG.md>`_

Wazuh core
----------

Most of the fixes introduced in this new version are focused on the user experience when dealing with the Wazuh management. Improving log messages and
configuration issues among other things. There are a few changes which are worth highlighting:

- Fixed a bug that prevented the remote upgrades for Ubuntu agents.
- An alert has been added to be aware when the process of unmerging the centralized configuration fails.
- Prevent interference between the Windows Defender antivirus and the Wazuh agent when managing temporary bookmark files.
- It is now possible to set up empty blocks of configuration for some modules. For example, the vulnerability detector module can be enabled by typing ``<wodle name="vulnerability-detector"/>``, applying it the default configuration for that module.

Wazuh API
---------

- The request to delete agents includes two new fields with the affected agents by the deletion request, as well as the failed IDs.
- Fixed error when trying to upgrade `Never connected` agents by the API.
