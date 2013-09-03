zabbix-disk-performance
=======================

Zabbix template for collecting IO statistics

With this template you can collect different disk statistics.

Installation
------------
To install, copy `userparameter_diskstats.conf` to /etc/zabbix/zabbix_agentd.d/userparameter_diskstats.conf and `lld-disks.py` to /var/lib/zabbix/scripts/lld-disks.py.
If /var/lib/zabbix/scripts does't exists, create it: ```sudo mkdir /var/lib/zabbix/scripts -p && sudo chown zabbix:zabbix /var/lib/zabbix -R```
`userparameter_diskstats.conf` is user parameters for Zabbix.
`lld-disks.py` is low level discovery script for enumerating disks of your system.

After that restart zabbix-agent
```sudo service zabbix-agent restart```

Go to Zabbix's web interface, Configuration->Templates and import `Template Disk Performance.xml`.
After that you should be able to monitor disk activity for all your disks.

Low level discovery won't list your RAID devices, so some tuning may be required.

Using without User Parameters
-----------------------------
Zabbix have [standard parameters](https://www.zabbix.com/documentation/2.0/manual/appendix/items/supported_by_platform) for monitoring disk io: `vfs.dev.read` and `vfs.dev.write` with several types:
* sectors
* operations
* sps
* ops

Template have this values configured, but disabled by default.


Testing
-------
To test that everything work use `zabbix_get`:
```
zabbix_get -s 127.0.0.1 -k "custom.vfs.dev.write.sectors[sda]"
```
