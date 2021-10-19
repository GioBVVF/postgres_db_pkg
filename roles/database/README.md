PostgresTask4Dba
=========

This code provides many tasks for activities on cluster database postgresql. It uses [Postgres Ansible module](https://docs.ansible.com/ansible/2.8/modules/list_of_database_modules.html) and is organized with [Ansible Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html).

Requirements
------------

* **new_database.yml** : Create a new database on a cluster and provide to set up also users to manage him. One user <newdbname>_web for application and <newdbname>_owner to administration. This playbook uses **psycopg2** pip module so it needs ** ansible_python_interpreter: "/usr/bin/python3 ** in eache file into **postgres_db_pkg\group_vars** directory. Operating System supported: **Linux CentOS 7**. ASAP i'll try to add also ubuntu distribution.

* **create_PG12cluster.yml** : Create a Potgres Cluser (Version 12) with 2 hot-standby and one witness-Server. Cluster uses [repmgr](https://repmgr.org/) tool. Witness-Server needs configuration for automatic-failover. This playbook uses **yum** ansible module so it needs ** ansible_python_interpreter: "/usr/bin/python2 ** in eache file into **postgres_db_pkg\group_vars** directory. Operating System supported: **Linux CentOS 7**. ASAP i'll try to add also ubuntu distribution.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

Twitter @gisaba1980
