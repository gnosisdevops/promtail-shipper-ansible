Promtail Shipper Ansible Role
=========

The Ansible Promtail Role allows you to effortlessly deploy and manage Promtail, agent which ships contents of local logs to private Loki.

## Requirements
------------

To be able to use the role one of these methods must be followed:

Install the role via ansible-galaxy command:

```
ansible-galaxy install git+https://github.com/gnosisdevops/promtail-shipper-ansible.git,main
```

Use a **requirements.yml** File
If you plan to use the role in multiple projects or ensure consistent dependency management:

Steps:

Create a requirements.yml:
```
---
roles:
  - name: promtail-shipper
    src: https://github.com/gnosisdevops/promtail-shipper-ansible.git
    version: main
```
Execute ansible-galaxy command:

```
ansible-galaxy install -r requirements.yml
```

Role Variables
--------------

```yaml
promtail_version: "latest"
```
The version of Promtail to download and deploy. Supported standard version "3.0.0" format or "latest".

```yaml
promtail_http_listen_port: 9080
```
The TCP port on which Promtail listens. By default, it listens on port `9080`.

```yaml
promtail_http_listen_address: "0.0.0.0"
```
The address on which Promtail listens for HTTP requests. By default, it listens on all interfaces.

```yaml
promtail_sync_period: 10s
```
Period of time which Promtail scrapes the logs.

```yaml
promtail_config_path: /etc/promtail/promtail-config.yml
```
Promtails default config path.

```yaml
loki_url: http://localhost:3100/loki/api/v1/push
```
Loki url which Promtail connects to. [All possible values for Loki `clients`](https://grafana.com/docs/loki/latest/clients/promtail/configuration/#clients). ⚠️ This configuration is mandatory.

```yaml
loki_username: []
```

```yaml
loki_password: []
```

Basic Auth used to access Loki endpoint. ⚠️ This configuration is mandatory.

```yaml
promtail_scrape_configs:
  - job_name: docker
    docker_sd_configs:
      - host: unix:///var/run/docker.sock
    relabel_configs:
      - source_labels: [__meta_docker_container_name]
        regex: '/(.*)'
        target_label: container
      - target_label: __address__
        replacement: localhost:8080
```
The `scrape_configs` block configures how Promtail can scrape logs from a series of targets using a specified discovery method. [All possible values for `scrape_configs`](https://grafana.com/docs/loki/latest/clients/promtail/configuration/#scrape_configs).


Example Playbook
----------------

Reference the Role in Your Playbook: [Requirements](#Requirements) Should be perform before
```yaml
---
- hosts: all
  gather_facts: true
  become: true
  tasks:
    - name: Install_promtail
      import_role:
        name: promtail-shipper
```

License
-------

BSD

Author Information
------------------

Gnosis
