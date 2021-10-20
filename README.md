## Ansible playbook for Postgres DBA

### Introduction

This code provides many tasks for activities on cluster database postgresql. It uses [Postgres Ansible module](https://docs.ansible.com/ansible/2.8/modules/list_of_database_modules.html) and is organized with [Ansible Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html). At the moment there's only two tasks :

* **new_database.yml** : Create a new database on a cluster and provide to set up also users to manage him. One user <newdbname>_web for application and <newdbname>_owner to administration. This playbook uses **psycopg2** pip module so it needs ** ansible_python_interpreter: "/usr/bin/python3 ** in eache file into **postgres_db_pkg\group_vars** directory. Operating System supported: **Linux CentOS**. ASAP i'll try to add also Ubuntu distribution.

* **create_PG_cluster.yml** : Create a Potgres Cluser (Version indicated in /var/cluster_params.yml) with 2 hot-standby and one witness-Server. Cluster uses [repmgr](https://repmgr.org/) tool. CLuster needs configuration for automatic-failover. This playbook uses **yum** ansible module so it needs ** ansible_python_interpreter: "/usr/bin/python2 ** in eache file into **postgres_db_pkg\group_vars** directory. Operating System supported: **Linux CentOS 7 and CentOS 8**. ASAP i'll try to add also Ubuntu distribution.

* **set_parameters.yml** : Set all parameters included in **roles\database\vars\set_parameters_var.yml** file where **required: True**. Setting is   registered in postgresql.auto.conf

* **add_replica_hotStandby.yml** : In progress ...
