# System Roles Demo WalkThru

## Requirements
* Minimum VM: 1vCPU x 1GB mem, running RHEL 7
* Multiple VMs can be used as targets for ansible inventory

## WalkThru:
* Installation, this occurs on the control node only.  It need not be installed on target nodes.
```
# yum --enablerepo=rhel-7-server-extras-rpms --enablerepo=rhel-7-server-ansible-2-rpms install rhel-system-roles ansible
```
* On the control node, create/edit ```/etc/ansible/hosts``` with the target host(s)
  * Or create an alternate inventory file and pass via ```-i``` cmdline argument
* Download and use the playbooks in this repository.
* Kdump
  * Run playbook ```example-kdump.yml``` to set kdump path to /var/crash
  * Validation: cat /etc/kdump.conf to see updates
* Network
  * Setup: Check connection name and substitute for 'eth0' below and edit playbook accordingly
  * Pre-check: ```nmcli con show eth0 | grep dns```
  * Run playbook ```example-network.yml``` to set DNS servers and domain search
  * Validation: ```nmcli con show eth0 | grep dns```
  * Cleanup: ```nmcli con mod eth0 ipv4.dns "" ipv4.dns-search""```
* SELinux
  * Setup: Change to permissive prior to exercise via ```setenforce 0```
  * Pre-check: ```sestatus```
  * Run playbook ```example-selinux.yml``` to set SELinux to ON and ENFORCING
  * Validation: ```sestatus```
* Timesync
  * Setup: Copy original conf file if cleanup is needed
  * Pre-check: ```grep -e ^server /etc/chrony.conf```
  * Run playbook ```example-timesync.yml``` to set NTP servers
  * Validation: ```grep -e ^server /etc/chrony.conf``` to see changes
  * Cleanup: Copy original conf file back to /etc/
