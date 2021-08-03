Ansible Packages Role
=====================

Role to install and configure [HAProxy](http://www.haproxy.org/).

Supported OS Families
---------------------

- Ubuntu (tested)
- Arch Linux (tested)
- Redhat (tested)
- Other Linux distributions should work just fine

Role Variables
--------------

Available variables are listed below, the default values are in [defaults/main.yml](./defaults/main.yml):
```
haproxy_package_name: name of haproxy package name, default to 'haproxy' (string)
haproxy_service_name: name of haproxy service name, default to 'haproxy' (string)

## Dict of haproxy global section, value can be string, int, list
haproxy_global_options: {}

## List of haproxy defaults section
haproxy_defaults_options:
- name: name of defaults, use "" to use defaults without name (string)
  options: dict of options for defaults section, value can be string, int, list, bool

## List of haproxy frontend section
haproxy_frontend_options:
- name: name of frontend (string)
  options: dict of options for frontend section, value can be string, int, list, bool

## List of haproxy backend section
haproxy_backend_options:
- name: name of backend (string)
  options: dict of options for backend section, value can be string, int, list, bool

## List of haproxy listen section
haproxy_listen_options:
- name: name of listen (string)
  options: dict of options for listen section, value can be string, int, list, bool

```

Dependencies
------------

None

Example Playbook
----------------

Here is an example playbook:
```
- hosts: all

  vars:
    haproxy_global_options:
      log:
      - /dev/log local0 debug
      - 127.0.0.1 local1 notice
      maxconn: 4096
      daemon: true
    haproxy_defaults_options:
    - name: ""
      options:
        log: global
	mode: http
    haproxy_frontend_options:
    - name: web
      options:
        bind: ":80"
	default_backend: test
    haproxy_backend_options:
    - name: test
      options:
        balance: roundrobin
	server:
	- myserver1 10.0.10.10:8000
	- myserver2 10.0.10.11:8000
    - name: test2
      options:
        balance: source
	server:
	- myserver1 10.0.10.10:8000
	- myserver2 10.0.10.11:8000

  roles:
  - budimanjojo.haproxy
```

License
-------

MIT License
