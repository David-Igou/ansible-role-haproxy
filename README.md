foo

# Ansible Role: HAProxy

Forked from https://github.com/geerlingguy/ansible-role-haproxy

[![CI](https://github.com/david-igou/ansible-role-haproxy/actions/workflows/ci.yml/badge.svg)](https://github.com/david-igou/ansible-role-haproxy/actions/workflows/ci.yml)

Installs HAProxy on RedHat/CentOS and Debian/Ubuntu Linux servers.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
haproxy_socket: "/var/run/haproxy.sock"
```
The socket through which HAProxy can communicate (for admin purposes or statistics). To disable/remove this directive, set `haproxy_socket: ''` (an empty string).

```yaml
haproxy_chroot: "/var/lib/haproxy"
```
The jail directory where chroot() will be performed before dropping privileges. To disable/remove this directive, set `haproxy_chroot: ''` (an empty string).

```yaml
haproxy_user: "haproxy"
haproxy_group: "haproxy"
```
The user and group under which HAProxy should run.

```yaml
haproxy_global_vars: []
```
A list of extra global variables to add to the global configuration section inside `haproxy.cfg`.

```yaml
haproxy_connect_timeout: "5000ms"
haproxy_client_timeout: "50000ms"
haproxy_server_timeout: "50000ms"
```
HAProxy default timeout configurations.

```yaml
haproxy_frontends:
  - name: "http-in"
    bind_address: "0.0.0.0"
    bind_port: 80
    mode: "http"
    default_backend: "servers"
    extra_options:
      - "option httplog"
      - "option http-server-close"
```
A list of frontend definitions. Each frontend requires a name, bind address/port, mode, default backend, and optional extra options.

```yaml
haproxy_backends:
  - name: "servers"
    mode: "http"
    balance_method: "roundrobin"
    extra_options: []
    servers:
      - name: "server1"
        address: "1.1.1.1:8080"
        options: []
      - name: "server2"
        address: "2.2.2.2:8080"
        options: []
```
A list of backend definitions. Each backend requires a name, mode, balance method, optional extra options, and a list of servers (with name, address, and options).

## Dependencies

None.

## Example Playbook

```yaml
- hosts: balancer
  become: yes
  roles:
    - role: David-Igou.haproxy
      vars: # minecraft backend, port 1337 frontend
        haproxy_frontends:
          - name: "minecraft-in"
            bind_address: "0.0.0.0"
            bind_port: 13337
            mode: "tcp"
            default_backend: "minecraft-backend"
            extra_options:
              - "option tcplog"
        haproxy_backends:
          - name: "minecraft-backend"
            mode: "tcp"
            servers:
              - name: "minecraft1"
                address: "192.168.2.55:25565"

MIT / BSD

## Author Information

This role was created in 2015 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).

This was forked in 2025 by [David Igou](https://www.github.com/david-igou).
