`elk-heartbeat-6.3.2`
=========

Role for installing Heartbeat 6.3.2.

Requirements
------------

Nothing specific outside other roles in this project. d(^_^)b

Role Variables
--------------

```aidl
heartbeat_version: "6.3.2"
heartbeat_home: "/usr/share/heartbeat"
heartbeat_conf_home: "/etc/heartbeat"
```

This role makes use of all variables in `group_vars/all` with the pattern `elasticsearch_*` for the connection to Elasticsearch.

Dependencies
------------

The following roles will be needed:
* `yum`
* `fix-NM`
* `elk-beats-certs`
* `elk-proxy-nginx`
* `openjdk`
* `elk-elasticsearch-6.3.2`

To be able to see anything get loaded from heartbeat, you might as well use the playbook
* `elk_install.yml`

Example Playbook
----------------

    ---
    - import_playbook: elk_install 

    - hosts: <elk hosts>, <targets>
      roles:
      - yum
      - elk-beats-certs
    
    - hosts: <elk hosts>, <targets>
      roles:
      - elk-heartbeat-6.3.2
    
>
### NOTE
There is a playbook, `elk-heartbeat_install.yml` which will run the beat install just fine for you. 

Also note that `preconfigure_environment.yml` will guarantee the install of
* ELK Stack
* Heartbeat

to ensure performance monitoring is set up and enabled even before MTC2 itself is installed.

Author Information
------------------

* Justin Shitanishi (justin.shitanishi.ctr@navy.mil)
