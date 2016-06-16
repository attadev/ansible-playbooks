Table of Contents

1. [Overview](#overview)
2. [Requirements for this role](#requirements)
   * [List of Operating systems](#operating-systems)
   * [Zabbix API Usage](#zabbix-api)
3. [Installing this role](#installation)
4. [Overview of variables which can be used](#role-variables)
   * [Main variables](#main-variables)
   * [Zabbix API Variables](#zabbix-api-variables)
4. [Dependencies](#dependencies)
5. [Example of using this role](#example-playbook)
   * [ Vars in role configuration](#vars-in-role-configuration)
   * [Combination of group_vars and playbook](#combination-of-group_vars-and-playbook)
7. [Extra information](#extra-information)
8. [License](#license)
9. [Author Information](#author-information)

#Overview


This is a role for installing/updating the zabbix-agent.

This is a modified copy of the 'gunkiniu' zabbix-agent role. See an complete list of roles:

 * (https://github.com/gunkiniu/ansible-zabbix-agent)
 
#Requirements
##Operating systems
This role will work on the following operating systems:

 * Red Hat
 * Debian
 * Ubuntu
 * suse/opensuse
 * Solaris 10

So, you'll need one of those operating systems.. :-)
Please sent Pull Requests or suggestions when you want to use this role for other Operating systems.

##Zabbix Versions

See the following list of supported Operating systems with the Zabbix releases:

Zabbix 3.0:

  * CentOS 5.x, 6.x, 7.x
  * Amazon 5.x, 6.x, 7.x
  * RedHat 5.x, 6.x, 7.x
  * OracleLinux 5.x, 6.x, 7.x
  * Scientific Linux 5.x, 6.x, 7.x
  * Ubuntu 14.04
  * Debian 7, 8

Zabbix 2.4:

  * CentOS 6.x, 7.x
  * Amazon 6.x, 7.x
  * RedHat 6.x, 7.x
  * OracleLinux 6.x, 7.x
  * Scientific Linux 6.x, 7.x
  * Ubuntu 12.04 14.04
  * Debian 7

Zabbix 2.2:

  * CentOS 5.x, 6.x
  * RedHat 5.x, 6.x
  * OracleLinux 5.x, 6.x
  * Scientific Linux 5.x, 6.x
  * Ubuntu 12.04
  * Debian 7
  * xenserver 6


## Zabbix API
When you want to automatically create the hosts in the webinterface, you'll need on your own machine the zabbix-api package. 

You can install this locally with the following command: `pip install zabbix-api`.

This ansible role uses these ansible extra modules: zabbix_group, zabbix_host and zabbix_hostmacro 

#Usage:

Example:

ansible-playbook -i roles/zabbix-agent/ansible_hosts roles/zabbix-agent/zabbix-agent.yml

#Role Variables

## Main variables
There are some variables in de default/main.yml which can (Or needs to) be changed/overriden:

* `agent_server`: The ipaddress for the zabbix-server or zabbix-proxy.

* `agent_serveractive`: The ipaddress for the zabbix-server or zabbix-proxy for active checks.

* `zabbix_version`: This is the version of zabbix. Default it is 2.4, but can be overriden to 2.2 or 2.0.

* `zabbix_repo`: Default: _epel_
  * _epel_ (default) install agent from EPEL repo
  * _zabbix_ install agent from Zabbix repo
  * _other_ install agent from pre-existing or other repo

* `agent_listeninterface`: Interface zabbix-agent listens on. Leave blank for all.

* `zabbix_agent_package_state`: If Zabbix-agent needs to be present or latest.

## Zabbix 3.0

These variables are specific for Zabbix 3.0/

* `agent_tlsconnect`: How the agent should connect to server or proxy. Used for active checks.

    Possible values:
    
    * unencrypted
    * psk
    * cert

* `agent_tlsaccept`: What incoming connections to accept.

    Possible values:
    
    * unencrypted
    * psk
    * cert

* `agent_tlscafile`: Full pathname of a file containing the top-level CA(s) certificates for peer certificate verification.

* `agent_tlscrlfile`: Full pathname of a file containing revoked certificates.

* `agent_tlsservercertissuer`: Allowed server certificate issuer.

* `agent_tlsservercertsubject`: Allowed server certificate subject.

* `agent_tlscertfile`: Full pathname of a file containing the agent certificate or certificate chain.

* `agent_tlskeyfile`: Full pathname of a file containing the agent private key.

* `agent_tlspskidentity`: Unique, case sensitive string used to identify the pre-shared key.

* `agent_tlspskfile`: Full pathname of a file containing the pre-shared key.

## Zabbix API variables
These variables needs to be changed/overriden when you want to make use of the zabbix-api for automatically creating and or updating hosts.

* `zabbix_url`: The url on which the Zabbix webpage is available. Example: http://zabbix.example.com

* `zabbix_api_use`: When you want to make use of the Zabbix-API. Default: False

* `zabbix_api_user`: Username of user which has API access.

* `zabbix_api_pass`: Password for the user which has API access.

* `zabbix_create_hostgroup`: present (Default) if you want to create hostgroups or absent if you not. 

* `zabbix_host_status`: enabled (Default) when host in monitored, disabled when host is disabled for monitoring.

* `zabbix_create_host`: present  # or absent

* `zabbix_useuip`: 1 if connection to zabbix-agent is made via ip, 0 for fqdn.

* `zabbix_host_groups`: An list of hostgroups which this host belongs to.

* `zabbix_link_templates`: An list of templates which needs to be link to this host. The templates should exist.

* `zabbix_macros`: An list with macro_key and macro_value for creating hostmacro's.


#Dependencies

There are no dependencies on other roles.

#Example Playbook
##Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - role: zabbix-agent
           agent_server: 192.168.33.30
           agent_serveractive: 192.168.33.30
           zabbix_url: http://zabbix.example.com
           zabbix_api_use: true
           zabbix_api_user: Admin
           zabbix_api_pass: zabbix
           zabbix_create_host: present
           zabbix_host_groups:
             - Linux Servers
           zabbix_link_templates:
             - Template OS Linux
             - Apache APP Template
           zabbix_macros:
             - macro_key: apache_type
               macro_value: reverse_proxy

##Combination of group_vars and playbook 
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: `group_vars/all` or `host_vars/<zabbix_server>` (Where <zabbix_server> is the hostname of the machine running Zabbix Server)

	agent_server: 192.168.33.30
        agent_serveractive: 192.168.33.30
        zabbix_url: http://zabbix.example.com
        zabbix_api_use: true
        zabbix_api_user: Admin
        zabbix_api_pass: zabbix
        zabbix_create_host: present
        zabbix_host_groups:
          - Linux Servers
        zabbix_link_templates:
          - Template OS Linux
          - Apache APP Template
        zabbix_macros:
          - macro_key: apache_type
            macro_value: reverse_proxy

and in the playbook only specifying:

    - hosts: all
      roles:
         - role: zabbix-agent

#Extra Information

You can install so-called userparameter files by adding the following into your roles:

```
- name: "Installing sample file"
  copy: src=sample.conf
        dest="{{ agent_include }}/mysql.conf"
        owner=zabbix
        group=zabbix
        mode=0755
  notify: restart zabbix-agent
```

Example of the "sample.conf" file:

```
UserParameter=mysql.ping_to,mysqladmin -uroot ping | grep -c alive
```

You can extend your zabbix configuration by adding items yourself that do specific checks which aren't in the zabbix core itself. You can change offcourse the name of the file to whatever you want (Same for the UserParameter line(s) ;-) 

(Maybe in near future doing it with variables.)

#License

GPLv3

#Author-Information

Original author: Github: https://github.com/dj-wasabi/ansible-zabbix-agent
The fork: Github: https://github.com/gunkiniu/ansible-zabbix-agent
