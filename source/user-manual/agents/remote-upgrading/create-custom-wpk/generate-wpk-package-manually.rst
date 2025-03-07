.. Copyright (C) 2022 Wazuh, Inc.

.. _create-custom-wpk-manually:

Generate WPK packages manually
==============================

WPK packages will generally contain the complete agent code, however, this is not required.

A WPK package must contain an installation program in binary form or a script in any language supported by the agent (Bash, Python, etc). Linux WPK packages must contain a Bash script named ``upgrade.sh`` for UNIX or ``upgrade.bat`` for Windows. This program must:

 * Fork itself, as the parent, will return 0 immediately.
 * Restart the agent.
 * Write a file called upgrade_result containing a status number (0 means OK) before exiting.

Requirements
^^^^^^^^^^^^

 * Python 2.7 or 3.5+
 * The Python ``cryptography`` package. This may be obtained using the following command:

.. code-block:: console

  $ pip install cryptography

Linux WPK
^^^^^^^^^

Install the development tools and compilers. In Linux, this can easily be done using your distribution package manager:

a) For RPM-based distributions:

.. code-block:: console

  # yum install make gcc policycoreutils-python automake autoconf libtool unzip

b) For Debian-based distributions:

.. code-block:: console

  # apt-get install make gcc libc6-dev curl policycoreutils automake autoconf libtool unzip

Download and extract the latest version:

.. code-block:: console

  # curl -Ls https://github.com/wazuh/wazuh/archive/v|WAZUH_CURRENT|.tar.gz | tar zx

Modify the ``wazuh-|WAZUH_CURRENT|/etc/preloaded-vars.conf`` file that was downloaded to deploy an :ref:`unattended update <unattended-installation>` in the agent by uncommenting the following lines:

.. code-block:: pkgconfig

  USER_LANGUAGE="en"
  USER_NO_STOP="y"
  USER_UPDATE="y"
  USER_BINARYINSTALL="y"

Compile the project from the ``src`` folder:

.. code-block:: console

  # cd wazuh-|WAZUH_CURRENT|/src
  # make deps TARGET=agent
  # make TARGET=agent

Delete the files that are no longer needed. This step can be skipped, but the size of the WPK will be considerably larger:

.. code-block:: console

  $ rm -rf doc wodles/oscap/content/* gen_ossec.sh add_localfiles.sh Jenkinsfile*
  $ rm -rf src/{addagent,analysisd,client-agent,config,error_messages,external/*,headers,logcollector,monitord,os_auth,os_crypto,os_csyslogd,os_dbdos_execd}
  $ rm -rf src/{os_integrator,os_maild,os_netos_regex,os_xml,os_zlib,remoted,reportd,shared,syscheckd,tests,update,wazuh_db,wazuh_modules}
  $ rm -rf src/win32
  $ rm -rf src/*.a
  $ rm -rf etc/{decoders,lists,rules}
  $ find etc/templates/* -maxdepth 0 -not -name "en" | xargs rm -rf

Install the root CA if you want to overwrite the root CA with the file you created previously:

.. code-block:: console

  # cd ../
  # cp path/to/wpk_root.pem etc/wpk_root.pem

Compile the WPK package using your SSL certificate and key:

.. code-block:: console

  # tools/agent-upgrade/wpkpack.py output/myagent.wpk path/to/wpkcert.pem path/to/wpkcert.key *

In this example, the Wazuh project's root directory contains the proper ``upgrade.sh`` file.

Definitions:
    - ``output/myagent.wpk`` is the name of the output WPK package.
    - ``path/to/wpkcert.pem`` is the path to the SSL certificate.
    - ``path/to/wpkcert.key`` is the path to the SSL certificate's key.
    - ``\*`` is the file or files to be included in the WPK package. In this case, all the contents are added.


Windows WPK
^^^^^^^^^^^

Install the development tools and compilers. In Linux, this can easily be done using your distribution package manager:

For RPM-based distributions:

.. code-block:: console

  # yum install make gcc policycoreutils-python automake autoconf libtool unzip

For Debian-based distributions:

.. code-block:: console

  # apt-get install make gcc libc6-dev curl policycoreutils automake autoconf libtool unzip

Download and extract the latest version of wazuh sources:

.. code-block:: console

  # curl -Ls https://github.com/wazuh/wazuh/archive/v|WAZUH_CURRENT|.tar.gz | tar zx

Download the latest version of the wazuh MSI package:

.. code-block:: console

  # curl -Ls https://packages.wazuh.com/|WAZUH_CURRENT_MAJOR_WINDOWS|/windows/wazuh-agent-|WAZUH_CURRENT_WINDOWS|-|WAZUH_REVISION_WINDOWS|.msi --output wazuh-agent-|WAZUH_CURRENT_WINDOWS|-|WAZUH_REVISION_WINDOWS|.msi

Install the root CA if you want to overwrite the root CA with the file you created previously:

.. code-block:: console

  # cd ../
  # cp path/to/wpk_root.pem etc/wpk_root.pem

Compile the WPK package using the MSI package and, your SSL certificate and key:

.. code-block:: console

  # tools/agent-upgrade/wpkpack.py output/myagent.wpk path/to/wpkcert.pem path/to/wpkcert.key path/to/wazuhagent.msi path/to/upgrade.bat path/to/do_upgrade.ps1

Definitions:
    - ``output/myagent.wpk`` is the name of the output WPK package.
    - ``path/to/wpkcert.pem`` is the path to the SSL certificate.
    - ``path/to/wpkcert.key`` is the path to the SSL certificate's key.
    - ``path/to/wazuhagent.msi`` is the path to the MSI file downloaded in step 3.
    - ``path/to/upgrade.bat`` is the path to the upgrade.bat file. Find an example in src/win32 in the Wazuh repository.
    - ``path/to/do_upgrade.ps1`` is the path to the do_upgrade.ps1 file. Find an example in src/win32 in the Wazuh repository.


macOS WPK
^^^^^^^^^

Install development tools and compilers. In Linux, this can easily be done using your distribution package manager:

For RPM-based distributions:

.. code-block:: console

  # yum install make gcc policycoreutils-python automake autoconf libtool unzip

For Debian-based distributions:

.. code-block:: console

  # apt-get install make gcc libc6-dev curl policycoreutils automake autoconf libtool unzip

Download and extract the latest version of Wazuh sources:

.. code-block:: console

  # curl -Ls https://github.com/wazuh/wazuh/archive/v|WAZUH_CURRENT|.tar.gz | tar zx

Download the latest version of the Wazuh PKG package:

.. code-block:: console

  # curl -Ls https://packages.wazuh.com/|WAZUH_CURRENT_MAJOR_OSX|/macos/wazuh-agent-|WAZUH_CURRENT_OSX|-|WAZUH_REVISION_OSX|.pkg --output wazuh-agent-|WAZUH_CURRENT_OSX|-|WAZUH_REVISION_OSX|.pkg

Install the root CA if you want to overwrite the root CA with the file you created previously:

.. code-block:: console

  # cd ../
  # cp path/to/wpk_root.pem etc/wpk_root.pem

Compile the WPK package using the PKG package and, your SSL certificate and key:

.. code-block:: console

  # tools/agent-upgrade/wpkpack.py output/myagent.wpk path/to/wpkcert.pem path/to/wpkcert.key path/to/wazuhagent.pkg path/to/upgrade.sh path/to/pkg_installer_mac.sh


Definitions:
    - ``output/myagent.wpk`` is the name of the output WPK package.
    - ``path/to/wpkcert.pem`` is the path to the SSL certificate.
    - ``path/to/wpkcert.key`` is the path to the SSL certificate's key.
    - ``path/to/wazuhagent.pkg`` is the path to the PKG file downloaded in step 3.
    - ``path/to/upgrade.sh`` is the path to the upgrade.sh file. Find an example at the base directory in the Wazuh repository.
    - ``path/to/pkg_installer_mac.sh`` is the path to the pkg_installer_mac.sh file. Find an example in src/init in the Wazuh repository.

.. note::
 These are only examples. If you want to distribute a WPK package using these methods, it's important to begin with an empty directory.
