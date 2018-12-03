Ubuntu 18.04 CIS
================

Configure Ubuntu 18.04 machine to be [CIS](https://www.cisecurity.org/cis-benchmarks/) compliant. Level 1 and 2 findings will be corrected by default.

This role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

Based on [CIS Ubuntu Linux 18.04 LTS Benchmark v1.0.0 - 08-13-2018 ](https://workbench.cisecurity.org/benchmarks/639).

This repo originated from work done by [Sam Doran](https://github.com/samdoran/ansible-role-stig)

Requirements
------------

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.
If you want to do a dry run without changing anything, set the below sections (u1804cis_section1-6) to false. 

Role Variables
--------------
There are many role variables defined in defaults/main.yml. This list shows the most important.

**u1804cis_notauto**: Run CIS checks that we typically do NOT want to automate due to the high probability of breaking the system (Default: false)

**u1804cis_section1**: CIS - General Settings (Section 1) (Default: true)

**u1804cis_section2**: CIS - Services settings (Section 2) (Default: true)

**u1804cis_section3**: CIS - Network settings (Section 3) (Default: true)

**u1804cis_section4**: CIS - Logging and Auditing settings (Section 4) (Default: true)

**u1804cis_section5**: CIS - Access, Authentication and Authorization settings (Section 5) (Default: true)

**u1804cis_section6**: CIS - System Maintenance settings (Section 6) (Default: true)  

##### Disable all selinux functions
`u1804cis_selinux_disable: false`

##### Service variables:
###### These control whether a server should or should not be allowed to continue to run these services

```
u1804cis_avahi_server: false  
u1804cis_cups_server: false  
u1804cis_dhcp_server: false  
u1804cis_ldap_server: false  
u1804cis_telnet_server: false  
u1804cis_nfs_server: false  
u1804cis_rpc_server: false  
u1804cis_ntalk_server: false  
u1804cis_rsyncd_server: false  
u1804cis_tftp_server: false  
u1804cis_rsh_server: false  
u1804cis_nis_server: false  
u1804cis_snmp_server: false  
u1804cis_squid_server: false  
u1804cis_smb_server: false  
u1804cis_dovecot_server: false  
u1804cis_httpd_server: false  
u1804cis_vsftpd_server: false  
u1804cis_named_server: false  
u1804cis_bind: false  
u1804cis_vsftpd: false  
u1804cis_httpd: false  
u1804cis_dovecot: false  
u1804cis_samba: false  
u1804cis_squid: false  
u1804cis_net_snmp: false  
```  

##### Designate server as a Mail server
`u1804cis_is_mail_server: false`


##### System network parameters (host only OR host and router)
`u1804cis_is_router: false`  


##### IPv6 required
`u1804cis_ipv6_required: true`  


##### AIDE
`u1804cis_config_aide: true`

###### AIDE cron settings
```
u1804cis_aide_cron:
  cron_user: root
  cron_file: /etc/crontab
  aide_job: '/usr/sbin/aide --check'
  aide_minute: 0
  aide_hour: 5
  aide_day: '*'
  aide_month: '*'
  aide_weekday: '*'  
```

##### SELinux policy
`u1804cis_selinux_pol: targeted` 


##### Set to 'true' if X Windows is needed in your environment
`u1804cis_xwindows_required: no` 


##### Client application requirements
```
u1804cis_openldap_clients_required: false 
u1804cis_telnet_required: false 
u1804cis_talk_required: false  
u1804cis_rsh_required: false 
u1804cis_ypbind_required: false 
```

##### Time Synchronization
```
u1804cis_time_synchronization: chrony
u1804cis_time_Synchronization: ntp

u1804cis_time_synchronization_servers:
    - 0.pool.ntp.org
    - 1.pool.ntp.org
    - 2.pool.ntp.org
    - 3.pool.ntp.org  
```  
  
##### 3.4.2 | PATCH | Ensure /etc/hosts.allow is configured
```
u1804cis_host_allow:
  - "10.0.0.0/255.0.0.0"  
  - "172.16.0.0/255.240.0.0"  
  - "192.168.0.0/255.255.0.0"    
```  

```
u1804cis_firewall: firewalld
u1804cis_firewall: iptables
``` 
  

Dependencies
------------

Ansible > 2.2

Example Playbook
-------------------------

This sample playbook should be run in a folder that is above the main u1804-CIS / u1804-CIS-devel folder.

```
- name: Harden Server
  hosts: servers
  become: yes

  roles:
    - u1804-CIS
```

Tags
----
Many tags are available for precise control of what is and is not changed.

Some examples of using tags:

```
    # Audit and patch the site
    ansible-playbook site.yml --tags="patch"
```

License
-------

MIT
