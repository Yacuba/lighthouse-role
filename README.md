# Ansible Role: lighthouse

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![GitHub tag](https://img.shields.io/github/v/tag/Yacuba/lighthouse-role)](https://github.com/Yacuba/lighthouse-role/tags)

## Description

Deploy and configure [LightHouse](https://github.com/VKCOM/lighthouse) — a lightweight web GUI client for ClickHouse database.

## Requirements

- **Ansible** >= 2.10
- **Target OS**:
  - Enterprise Linux (RHEL / AlmaLinux / Rocky Linux / CentOS 8/9)
- **Dependencies**: An operational web server (e.g. Nginx) is expected on the host to serve the static content.

## Role Variables

All variables that can be overridden are stored in [defaults/main.yml](defaults/main.yml).

### Default Variables (`defaults/main.yml`)

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `lighthouse_version` | `"master"` | Git branch or release tag of LightHouse to install. |
| `lighthouse_url` | `"https://github.com/VKCOM/lighthouse/archive/refs/heads/master.tar.gz"` | Source archive download URL. |
| `lighthouse_dir` | `"/var/www/lighthouse"` | Document root directory on target host. |
| `lighthouse_port` | `80` | HTTP listen port for Nginx virtual host. |
| `lighthouse_server_name`| `"_"` | Domain name or wildcard for `server_name` directive. |

### Internal Variables (`vars/main.yml`)

| Variable | Value | Description |
| :--- | :--- | :--- |
| `lighthouse_archive_dest` | `"/tmp/lighthouse-{{ lighthouse_version }}.tar.gz"` | Local path to download tarball. |
| `lighthouse_nginx_conf_dest` | `"/etc/nginx/conf.d/lighthouse.conf"` | Nginx site configuration target file. |
| `lighthouse_user` | `"nginx"` | Owner user for web root directory. |
| `lighthouse_group` | `"nginx"` | Owner group for web root directory. |

## Dependencies

None. Web server (Nginx) installation should be handled by the playbook or a separate web server role.

## Example Playbook

An example of combining LightHouse role with pre-tasks to ensure Nginx is installed:

```yaml
---
- name: Install and configure Lighthouse
  hosts: lighthouse
  pre_tasks:
    - name: Install EPEL and Nginx
      become: true
      ansible.builtin.dnf:
        name:
          - epel-release
          - nginx
        state: present
  roles:
    - role: lighthouse-role
```

## Handlers

* `Reload Nginx service`
* `Restart Nginx service`

## License

This project is licensed under the MIT License.

## Author Information

Created by [Yacuba](https://github.com/Yacuba).