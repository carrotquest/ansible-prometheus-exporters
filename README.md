# ansible-prometheus-exporters role

Download, setup and run various Prometheus exporters as systemd unit for VM and BareMetal servers.

## Exporters

### Default

- [node_exporter](https://github.com/prometheus/node_exporter)

### Optional

- [elasticsearch_exporter](https://github.com/justwatchcom/elasticsearch_exporter)
- [pgbouncer_exporter](https://github.com/jbub/pgbouncer_exporter)
- [postgres_exporter](https://github.com/wrouesnel/postgres_exporter)
- [rabbitmq_exporter](https://github.com/kbudde/rabbitmq_exporter)
- [redis_exporter](https://github.com/oliver006/redis_exporter)
- [nginx_exporter](https://github.com/nginxinc/nginx-prometheus-exporter)
- [json_exporter](https://github.com/prometheus-community/json_exporter)


### Template for sample_exporter

```yaml
---
## sample_exporter
exporter:
  ## uri default combined as: "{{ item.repo }}/releases/download/v{{ item.version }}/{{ item.name }}-{{ item.version }}.{{ item.platform }}.tar.gz"
  ## uri with package name combined as: "{{ item.repo }}/releases/download/v{{ item.version }}/{{ item.package }}.tar.gz"
  ## binary_path default combined as:
  ##   "{{ item.name }}-{{ item.version }}.{{ item.platform }}/{{ item.name }}" without package and single=false
  ##   "{{ item.package }}/{{ item.name }}" with package and single=false
  ##   "{{ item.name }}" if single=true an
  - name: # sample_exporter
    description: # 'Sample metric exporter for Prometheus'
    alias: # sample-exporter
    run_user: # sample run user, default is 'prometheus'
    repo: # sample repo url https://github.com/sample/sample_exporter
    package: # sample_exporter archive name if conflict with uri default combined
    binary_path: # sample_exporter binary path inside archive if conflict with binary_path default combined
    single: # yes if single binary file
    version: # sample_exporter version
    platform: # linux-amd64
    port: # 9XXX
```

## Requirements

Ubuntu Xenial/Bionic with systemd

## Role Variables

```yaml
---
consul_host: 127.0.0.1
consul_path: "/opt/consul/conf.d"
exporters_consul_enabled: no

exporters_tmp_dir: "/tmp/prometheus_exporters"
exporters_bin_dir: "/usr/local/sbin"
exporters_run_user: "prometheus"

default_exporters:
  - name: node_exporter
    description: "Machine metrics exporter for Prometheus"
    alias: node-exporter
    repo: https://github.com/prometheus/node_exporter
    version: 0.18.0
    platform: linux-amd64
    port: 9100
```

## Dependencies

- Ansible 2.4+
- systemd

## Example Playbook

```yaml
---
- hosts: servers
  roles:
    - role: ansible-prometheus-exporters
      vars:
        services_list:
          - elasticsearch
          - pgbouncer
          - postgres
          - rabbitmq
          - redis
          - nginx
```

## License

MIT License

## Author Information

Carrol Cox <carrol@protonmail.com>
