ansible-tower-setup
=========

A role to configure multi node Tower installation with an option to configure
database nodes

This role installs the Ansible Tower setup playbook and appends Sam Doran's
postgres replication role for configuring streaming replication if needed.

Requirements
------------

* Ansible 2.3+
* RHEL/Centos Nodes.
* Ubuntu Latest LTS

Role Variables
--------------

* ``ansible_tower_version`` **(required)**: Set the tower version you wish to install. This is blank by default.

* ``tower_servers`` **(required)**: List of DNS names of Ansible Tower servers

* ``postgres_server`` **(required)**: List of DNS names of Postgres servers. List only **2**
  servers. The first server is the master server. The 2nd server is a slave server.

* ``postgres_streaming_replication``: Defaults to ``false``. If set to true assumes
  you want to this role to create a pair of postgres database servers with
  streaming replication enabled. If using this role on a AWS setup, leave it as False as AWS RDS provides its own HA.

* ``use_bundle``: if set to true it will download the Ansible tower bundle

Dependencies
------------

None

Example Playbook
----------------

```
  - hosts: tower_install
    roles:
      - role: setup-tower
        ansible_tower_version: 3.1.3
        tower_servers:
          - tower1.example.com
        postgres_servers:
          - postgres1.example.com
```

License
-------

MIT

Author Information
------------------

Stanley Karunditu (stanley at linuxsimba dot com )
