Alignak checks package for NRPE
======================================

Checks pack for monitoring hosts with NRPE


Installation
----------------------------------------

From PyPI
~~~~~~~~~~~~~~~~~~~~~~~
To install the package from PyPI:
::
   pip install alignak-checks-nrpe


From source files
~~~~~~~~~~~~~~~~~~~~~~~
To install the package from the source files:
::
   git clone https://github.com/Alignak-monitoring-contrib/alignak-checks-nrpe
   cd alignak-checks-nrpe
   mkdir /usr/local/etc/alignak/arbiter_cfg/objects/packs/nrpe
   # Copy configuration files
   cp -R alignak_checks_nrpe/*.cfg /usr/local/etc/alignak/arbiter_cfg/objects/packs/nrpe
   # Copy plugin files
   cp -R alignak_checks_nrpe/plugins/* /usr/local/libexec/alignak


Documentation
----------------------------------------

Configuration
~~~~~~~~~~~~~~~~~~~~~~~

Install the NRPE plugin from Nagios
::
   # Install local NRPE plugin
   apt-get install nagios-nrpe-plugin
   # Note: This installs all the Nagios stuff on your machine ...


Prepare host
~~~~~~~~~~~~~~~~~~~~~~~
Some operations are necessary on the monitored hosts if SNMP remote access is not yet activated.
::
   # Install local NRPE server
   su -
   apt-get update
   apt-get install nagios-nrpe-server
   apt-get install nagios-plugins

   # Allow remote host
   vi /etc/nagios/nrpe.cfg
   =>
      allowed_hosts = X.X.X.X

   # Restart SNMP agent
   /etc/init.d/nagios-nrpe-server start

Test remote access with the plugins files:
::



Alignak configuration
~~~~~~~~~~~~~~~~~~~~~~~

You simply have to tag the concerned hosts with the template `linux-snmp`. The main `linux-snmp` template declares macros used to configure the launched checks. The default values of these macros listed hereunder can be overriden in each host configuration.
::
   _SNMPCOMMUNITY      $SNMPCOMMUNITYREAD$
   _SNMP_MSG_MAX_SIZE  65535

   _LOAD_WARN          2,2,2
   _LOAD_CRIT          3,3,3
   _STORAGE_WARN       90
   _STORAGE_CRIT       95
   _CPU_WARN           80
   _CPU_CRIT           90
   _MEMORY_WARN        80
   _MEMORY_CRIT        95
   _NET_IFACES         eth\d+|em\d+
   _NET_WARN           90,90,0,0,0,0
   _NET_CRIT           0,0,0,0,0,0


To set a specific value for an host, declare the same macro in the host definition file.
::
   define host{
      use                     linux-snmp
      contact_groups          admins
      host_name               sim-vm-snmp
      address                 192.168.0.18

      # Specific values for this host
      # Change warning and critical alerts level for memory
      # Same for CPU, ALL_CPU, DISK, LOAD, NET, ...
      _LOAD_WARN       3,3,3
      _LOAD_CRIT       5,5,5
   }


Bugs, issues and contributing
----------------------------------------

Contributions to this project are welcome and encouraged ... issues in the project repository are the common way to raise an information.

License
----------------------------------------

Alignak Pack EXAMPLE is available under the `GPL version 3 license`_.

.. _GPL version 3 license: http://opensource.org/licenses/GPL-3.0