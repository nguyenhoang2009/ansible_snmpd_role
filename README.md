# SNMPD Ansible Role

Based on https://github.com/Oefenweb/ansible-snmpd without the user auth thing and more things.

Import in playbook:
```yaml
- hosts: all
  roles:
      - { role: rhaamo.snmpd, become: true }
```

Default config:
```yaml
---

# Default packages for snmpd
snmpd_debian_packages:
  - snmp
  - snmpd
  - snmp-mibs-downloader
snmpd_redhat_packages:
  - net-snmp
  - net-snmp-utils

# Should we start and enable the daemon
snmpd_enabled: True

# MIBs to load
snmpd_mibs: UCD-SNMP-MIB

# Default SNMPD options (use syslog, close stdin/out/err)
snmpd_opts: '-LS4d -Lf /dev/null -u snmp -g snmp -I -smux -p /var/run/snmpd.pid'

# Should we start and enable snmptrapd
snmpd_trapd_enabled: false

# Default options for snmptrapd (use syslog)
snmpd_trapd_opts: '-Lsd -p /var/run/snmptrapd.pid'

# Create a symlink on debian legacy location to official RFC path
snmpd_snmpd_compat: false

# Default listen on everything
snmpd_agent_address:
  - 'udp:161'
  - 'udp6:[::1]:161'

# List of networks to authorize
# snmpd_authorized_networks:
#   - community: public
#     network: 192.168.40.0/32
snmpd_authorized_networks: []

# System location
snmpd_sys_location: 'Unknown'

# System contact
snmpd_sys_contact: Root <root@localhost>

# System description, defaults on inventory hostname
snmpd_sys_description: "{{ inventory_hostname }}"

# Include all disks mounted on system
snmpd_disks_include_all: false
# Threshold for all disks mounted
snmpd_disks_include_all_threshold_minpercent: '10%'

# List of disks
# snmpd_disks:
#   - path: /dev/sda
#     threshold: 69%
snmpd_disks: []

# Configure the Event MIB tables to monitor the various UCD-SNMP-MIB tables for problems
snmpd_default_monitors: true
# Configure the Event MIB tables to monitor the fTable for network interface being taken up or down, and triggering a linkUp or linkDown notification as appropriate
snmpd_link_up_down_notifications: true

# List of SNMPD extensions
# snmpd_extensions:
#   - name: farts
#     prog: /usr/local/bin/yolo
snmpd_extensions: []
```