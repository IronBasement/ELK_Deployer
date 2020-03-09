`elk-kibana-6.3.2`
=========

Role for installing Kibana 6.3.2.

Requirements
------------

Nothing specific outside other roles in this project. d(^_^)b

While this role itself may run, nothing will be able to connect to it. That is what the `elk-proxy-nginx` role is for. See below.

Role Variables
--------------

```aidl
kibana_version: "6.3.2"
```

This role makes use of all variables in `group_vars/all` with the pattern `kibana_*`, as well as `elasticsearch_*` variables for the connection to Elasticsearch.

Dependencies
------------

The following roles will be needed:
* `yum`
* `fix-NM`
* `elk-beats-certs`
* `elk-proxy-nginx`
* `elk-elasticsearch-6.3.2`

Example Playbook
----------------

    - hosts: <kibana host>, <elasticsearch host>
      roles:
      - yum
      - fix-NM
      - elk-beats-certs
      
    - hosts: <elasticsearch host>
      roles:
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

    - hosts: <kibana host>
      tasks:
        - name: "Set up NGINX proxy in front of Kibana"
          include_role:
            name: elk-proxy-nginx
          vars:
            nginx_basic_auth_user: "{{ kibana_username }}"
            nginx_basic_auth_password: "{{ kibana_password }}"
            nginx_basic_auth_user_file_name: ".login-kibana"

    - hosts: <kibana host>
      roles:
      - elk-kibana-6.3.2
>
### NOTE
There is a playbook, `elk_install.yml` which will run this just fine for you. 

Author Information
------------------

* Justin Shitanishi (justin.shitanishi.ctr@navy.mil)
