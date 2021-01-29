# Ansible ElasticSearch role

Install ElasticSearch, configure a cluster and user security and ssl communication between nodes. Install using apt-get.

### Requirements
* Ubuntu 20.04
* Ansible 2.10
* Python 3

### Variables
```yaml
elastic_version: "7.10.2"
elastic_user: elasticsearch
elastic_group: elasticsearch

elastic_config_dir: /etc/elasticsearch
elastic_log_dir: /var/log/elasticsearch
elastic_home_dir: /usr/share/elasticsearch
elastic_cert_dir: "{{ elastic_config_dir }}/certs"
elastic_bin_dir: "{{ elastic_home_dir }}/bin"
elastic_data_dir: /var/log/elasticsearch
elastic_bind_host: "{{ ansible_host }}"

user_home_dir: /home/{{ ansible_user }}

elastic_http_port: 9200
elastic_transport_port: 9300

elastic_cluster_enable: false # enable cluster
# hosts in cluster
# {{ groups.elastic | map('extract', hostvars, 'ansible_host') | product([transport_port]) | map('join', ':') }}
elastic_discovery_hosts: []
elastic_node_name: "{{ inventory_hostname }}" # node name, usually hostname
elastic_node_roles: # node roles
  - master
  - data

elastic_cluster_name: es_cluster # default cluster name
# Nodes for initial cluster configuration
# {{ groups.elastic | map('extract', hostvars, 'inventory_hostname') | list }}
elastic_cluster_nodes: "{{ inventory_hostname }}"

elastic_roles:
  admin:
    cluster:
      - all
    indices:
      - names: "*"
        privileges:
          - all

elastic_users:
  - name: admin
    password: root
    roles:
      - admin

elastic_java_memory: 1g # Java -Xmx -Xms memory
```