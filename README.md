# Configuration on Proxmox

### Install snmpd
`apt install snmpd`

### Configure snmpd
#### Paste content from snmpd.conf to /etc/snmp/snmpd.conf
`nano /etc/snmp/snmpd.conf`

### Paste content from sh file to /opt folder and make executable
`nano /opt/snmp-cpu-temp.sh`<br/>
`nano /opt/snmp-smart-status.sh`<br/>
`nano /opt/snmp-lvm-used.sh` for LVM<br/>
`nano /opt/snmp-zfs-used.sh` for ZFS<br/>
`nano /opt/snmp-ceph-used.sh` for Ceph<br/>
`chmod +x /opt/snmp-*`

### Run snmpd as root, because debian added the user "Debian-snmp" to the snmp.service but for SMART/LVM Status we need to be root.
#### edit snmpd.service and replace "Debian-snmp" with "root"
`nano /lib/systemd/system/snmpd.service`
##### From
```
ExecStart=/usr/sbin/snmpd -Lsd -Lf /dev/null -u Debian-snmp -g Debian-snmp -I -smux,mteTrigger,mteTriggerConf -f -p /run/snmpd.pid
```
##### To
```
ExecStart=/usr/sbin/snmpd -Lsd -Lf /dev/null -u root -g Debian-snmp -I -smux,mteTrigger,mteTriggerConf -f -p /run/snmpd.pid
```
#### Restart SNMPD
`systemctl restart snmpd.service`
