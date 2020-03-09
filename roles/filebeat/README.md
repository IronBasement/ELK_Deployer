`elk-filebeat-6.3.2`
=========

Role for installing Filebeat 6.3.2.

Requirements
------------

Nothing specific outside other roles in this project. d(^_^)b

Role Variables
--------------

```aidl
filebeat_version: "6.3.2"

filebeat_home: "/usr/share/filebeat"
filebeat_conf_home: "/etc/filebeat"
filebeat_conf_modules_subidr: "modules.d"
filebeat_conf_modules: "{{ filebeat_conf_home }}/{{ filebeat_conf_modules_subdir }}"

filebeat_kibana_dashboards_subdir: "kibana/6/dashboard"
filebeat_kibana_dashboards_dir: "{{ filebeat_home }}/{{ filebeat_kibana_dashboards_subdir }}"
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
    * Elasticsearch will need the `ingest-geoip` for the `system` module for Filebeat to fully work, and the above role will do that for you.

The playbook `elk_install.yml` will help with getting you set up with the necessary deployment dependencies.

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
      - elk-filebeat-6.3.2
    
    - hosts: <kibana host>
      tasks:
      - name: "Import Metricbeat Dashboards"
        command: "/bin/filebeat setup --dashboards"
>
### NOTE
There is a playbook, `elk-filebeat_install.yml` which will run the beat install just fine for you. 

Also note that `preconfigure_environment.yml` will guarantee the install of
* ELK Stack
* Metricbeat

to ensure performance monitoring is set up and enabled even before MTC2 itself is installed.

The logs captured by Filebeat so far:
* Files matching the pattern `/var/logs/*.log`
* EnterpriseDB logs

Author Information
------------------

* Justin Shitanishi (justin.shitanishi.ctr@navy.mil)
