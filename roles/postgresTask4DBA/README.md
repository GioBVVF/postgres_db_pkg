postgresTask4DBA
=========

This roles provides many tasks for activities on cluster database postgresql. It uses [Postgres Ansible module](https://docs.ansible.com/ansible/2.8/modules/list_of_database_modules.html) and is organized with [Ansible Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html).

Requirements
------------

* **new_database.yml** : Create a new database on a cluster and provide to set up also users to manage him. One user <newdbname>_web for application and <newdbname>_owner to administration. This playbook uses **psycopg2** pip module so it needs ** ansible_python_interpreter: "/usr/bin/python3 ** in eache file into **postgres_db_pkg\group_vars** directory. Operating System supported: **Linux CentOS 7**. ASAP i'll try to add also ubuntu distribution.

* **create_PG_cluster.yml** : Create a Potgres Cluser (Version indicated in /var/cluster_params.yml) with 2 hot-standby and one witness-Server. Cluster uses [**repmgr**](https://repmgr.org/) tool. CLuster needs configuration for automatic-failover. This playbook uses **yum** ansible module so it needs ** ansible_python_interpreter: "/usr/bin/python2 ** in eache file into **postgres_db_pkg\group_vars** directory. Operating System supported: **Linux CentOS 7 and CentOS 8**. ASAP i'll try to add also Ubuntu distribution. It uses **roles\database\vars\cluster_params.yml** file. This playbook it can also be used to add nodes to the cluster one at a time or add another replica. To do that you have to run [**ansible-playbook**](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html) commando with **--limit** parameter and specify **group** i.e. **sudo ansible-playbook playbook.yml -i inventory.ini --limit pg_slaves --ask-become-pass** and in inventory.ini file in relative group, uncomment only hosts that need added on cluster. It uses **roles\database\vars\cluster_params.yml** file.

* **set_parameters.yml** : Set all parameters included in **roles\database\vars\set_parameters_var.yml** file where **required: True**. Setting is   registered in postgresql.auto.conf

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

* \postgresTask4DBA\vars\cluster_params.yml :

```
repplication_user: repmgr      # replication user used for manage repmgr
replication_db: repmgr         # replication db for repmgr
postgres_version: 14
#repmgr_version: 14
data_dir_postgres: '/var/lib/pgsql/{{postgres_version}}/data/' # Postgres data directory on CentOS
primary_host: 192.168.1.181 # Master cluster from hotstanby will be cloned
mount_point_tbs : /pgsql_tsds/pgsql  # If use tablespaces you can indicate here mounting point
wal_level_value : replica # Wal level must be replica in MAster-HotStanby configuration
network_segment: 192.168.0.0 # Network where cluster's nodes are installed
packages: # Packages that must be installed
  - name: 'postgresql{{postgres_version}}-server'
    required: True
  - name:  'postgresql{{postgres_version}}-contrib'
    required: True
  - name:  'repmgr_{{postgres_version}}'
    required: True
nodes: # Here describe cluster's node and variables that must be used for repmgr configuration
  - name: PG_master
    node_id: 1
    host_name: PG_master_1
    conninfo: 'conninfo=''host={{ansible_host}} user={{repplication_user}} dbname={{replication_db}} connect_timeout=2'''
    data_directory: 'data_directory=''{{data_dir_postgres}}'''
    replication_slots: 'use_replication_slots=true'
  - name: PG_standby1
    node_id: 2
    host_name: pg_slave_1
    conninfo: 'conninfo=''host={{ansible_host}} user={{repplication_user}} dbname={{replication_db}} connect_timeout=2'''
    data_directory: 'data_directory=''{{data_dir_postgres}}'''
    replication_slots: 'use_replication_slots=true'
  - name: PG_standby2
    node_id: 3
    host_name: pg_slave_2
    conninfo: 'conninfo=''host={{ansible_host}} user={{repplication_user}} dbname={{replication_db}} connect_timeout=2'''
    data_directory: 'data_directory=''{{data_dir_postgres}}'''
    replication_slots: 'use_replication_slots=true'
  - name: PG_witness
    node_id: 4
    host_name: PG_witness
    conninfo: 'conninfo=''host={{ansible_host}} user={{repplication_user}} dbname={{replication_db}} connect_timeout=2'''
    data_directory: 'data_directory=''{{data_dir_postgres}}'''
    replication_slots: 'use_replication_slots=true'
```

Dependencies
------------

Python must be installed

Example Playbook
----------------

Including an example of how to use your role:

```
-
  name: 'Postgres cluster database operations for DBA by Giovanni Barbato'
  hosts: all
  become: yes
  become_method: sudo
  gather_facts: yes
  vars:
  vars_files:
    - ./roles/postgresTask4DBA/vars/new_database_var.yml
    #- ./roles/postgresTask4DBA/vars/cluster_params.yml
    - ./roles/postgresTask4DBA/vars/set_parameters_var.yml
  roles:
    - postgresTask4DBA
```

License
-------

BSD

Author Information
------------------

[Giovanni Barbato](https://github.com/GioBVVF)
