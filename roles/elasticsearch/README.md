`elk-elasticsearch-6.3.2`
=========

Role for installing Elasticsearch 6.3.2.

Requirements
------------

Nothing specific outside other roles in this project. d(^_^)b

While this role itself may run, nothing will be able to connect to it. That is what the `elk-proxy-nginx` role is for. See below.

In order for the Elasticsearch instance installed and configured via this role to support Filebeat's (6.3.2) `system` module, a plugin for Elasticsearch (`ingest-geoip`) is required specifically for the `system` module's `auth` tracking features. The plugin is installed directly from a `.zip` file and the necessary configurations for it are tracked within the role `artifacts-mtc2`. 

Role Variables
--------------

```aidl
elasticsearch_version: "6.3.2"
elasticsearch_home: "/usr/share/elasticsearch"
elasticsearch_conf_home: "/etc/elasticsearch"
```

This role makes use of all variables in `group_vars/all` with the pattern `elasticsearch_*`.

Dependencies
------------

The following roles will be needed:
* `yum`
* `fix-NM`
* `elk-beats-certs`
* `openjdk`
* `elk-proxy-nginx`

This role will internally call the following roles:
* `artifacts-mtc2` to properly stage the `ingest-geoip.zip` mentioned above in **Requirements**.

Example Playbook
----------------

    - hosts: <elasticsearch host>
      roles:
      - yum
      - fix-NM
      - elk-beats-certs
      - openjdk

    - hosts: <elasticsearch host>
      tasks:
        - name: "Set up NGINX proxy in front of Elasticsearch"
          include_role:
            name: elk-proxy-nginx
          vars:
            nginx_basic_auth_user: "{{ elasticsearch_username }}"
            nginx_basic_auth_password: "{{ elasticsearch_password }}"
            nginx_basic_auth_user_file_name: ".login-elasticsearch"

    - hosts: <elasticsearch host>
      roles:
      - elk-elasticsearch-6.3.2

### NOTE
There is a playbook, `elk_install.yml` which will run this just fine for you.

    ---
    - import_playbook: elk_install.yml

Author Information
------------------

* Justin Shitanishi (justin.shitanishi.ctr@navy.mil)
