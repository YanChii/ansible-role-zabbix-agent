
## Role
Instals zabbix agent and configures additional checks if requested.

## Supported OSes
- Redhat-like 7, 8
- Ubuntu
- Debian

## Requirements
To use this role, you need to have locally installed `community.zabbix` collection and `zabbix-api` python module (see [https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html](module requirements)). In short:
```
ansible-galaxy collection install community.zabbix
sudo apt install python3-pip
sudo pip3 install zabbix-api
```

## Required vars
- `zabbix_agent_zabbix_server` - comma separated list of zabbix servers (IP or hostname)
- `zabbix_agent_server_login_pass` - required if push host to zabbix server is requested

## Opional vars
- `zabbix_agent_version` - default: `5.0`
- `zabbix_agent_repo_pkg_url` - override package that sets up zabbix repository (e.g. `https://repo.zabbix.com/zabbix/5.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.2-1+ubuntu20.04_all.deb`)
- `zabbix_agent_extra_checks` - list of checks to add into the config file
- `zabbix_agent_force_repo_upgrade` - if you change `zabbix_agent_version`, force update of the repo package to apply the change (default: `false`)
- `zabbix_agent_force_pkg_upgrade` - try to upgrade zabbix-agent package on every run if possible (default: value of `zabbix_agent_force_repo_upgrade`)
- `zabbix_agent_add_host_to_server` - add host to zabbix server (requires zabbix server credentials) (default: true if `zabbix_agent_server_url` is not none)
- `zabbix_agent_server_url` - zabbix server url that will be contacted to add host using API (default: none)
- `zabbix_agent_server_login_user` - (default: `Admin`)
- `zabbix_agent_proxy` - specify zabbix proxy that will monitor current host (default: none)
- `zabbix_agent_hostgroups` - add host to specified hostgroups on Zabbix server
- `zabbix_agent_host_templates` - assing specified host templates on Zabbix server

## Example playbook
```
- hosts: zabbix-agents
  become: yes
  vars:
    zabbix_agent_zabbix_server: 10.20.30.40
    zabbix_agent_version: 5.3
    zabbix_agent_force_repo_upgrade: false
    zabbix_agent_force_pkg_upgrade: true
    zabbix_agent_hostgroups:
      - linuxes
      - my_other_hosts
  roles:
    - zabbix-agent
```

## Known issues

Currently assigning `zabbix_agent_host_templates` is failing with latest Zabbix (`v5.3`).

