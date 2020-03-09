`elk-metricbeat-6.3.2`
=========

Role for installing Metricbeat 6.3.2.

Requirements
------------

Nothing specific outside other roles in this project. d(^_^)b

Role Variables
--------------

```aidl
metricbeat_version: "6.3.2"
metricbeat_home: "/usr/share/metricbeat"
metricbeat_conf_home: "/etc/metricbeat"
metricbeat_conf_modules_subidr: "modules.d"
metricbeat_conf_modules: "{{ metricbeat_conf_home }}/{{ metricbeat_conf_modules_subdir }}"

metricbeat_kibana_dashboards_subdir: "kibana/6/dashboard"
metricbeat_kibana_dashboards_dir: "{{ metricbeat_home }}/{{ metricbeat_kibana_dashboards_subdir }}"
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

To be able to see anything get loaded from metricbeat, you might as well use the playbook
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
      - elk-metricbeat-6.3.2
    
    - hosts: <kibana host>
      tasks:
      - name: "Import Metricbeat Dashboards"
        command: "/bin/metricbeat setup --dashboards"
>
### NOTE
There is a playbook, `elk-metricbeat_install.yml` which will run the beat install just fine for you. 

Also note that `preconfigure_environment.yml` will guarantee the install of
* ELK Stack
* Metricbeat

to ensure performance monitoring is set up and enabled even before MTC2 itself is installed.

Author Information
------------------

* Justin Shitanishi (justin.shitanishi.ctr@navy.mil)
