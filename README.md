# zabbix.importer.csv
Import Zabbix hosts from CSV

Setup
----
```
curl -L ansible.bitbull.ch | bash
pip3 install zabbix-api zabbix-cli
ansible-galaxy collection install community.zabbix
cd /etc/ansible/projects/
git clone git@github.com:joe-speedboat/zabbix.importer.csv.git
```

CSV FORMAT
---
* the first row of csv defines the variable names within ansible. Do not touch this line
  * srv_name
    * mandatory
  * srv_ip
    * mandatory
  * srv_dns
    * optional
  * srv_check_type: [agent|snmp]
    * default: agent
    * optional
  * srv_snmp_comunity
    * default: {$SNMP_COMMUNITY}
    * optional
  * srv_desc
    * optional
      note: short names, no special chars
  * srv_tag
    * Always in: csv:{{ csv_path }}
    * optional 
      eg: tag1,tag2:99,tag3:value3
      note: short names, no special chars
            if alert:none -> srv_location will be modified from srv_location to noAlert/srv_location or Alert/srv_location 
            they get asked on run:  maintain_alert_tags -> no_alert_tag -> no_alert_location_prefix, alert_location_prefix
  * srv_location
    * optional
      eg: StGallen
      note: short names, no special chars
  * srv_hostgroup
    * Always in: csv
    * optional
      eg: Group1,Group2,Group3,...
      note: short names, no special chars
  * srv_template
    Zero or multiple Templates to link
    eg: TPL1,TPL2,TPL3,...
    default templates are predefined in .yml

```
srv_name;srv_ip;srv_dns;srv_check_type;srv_snmp_comunity;srv_desc;srv_tag;srv_location;srv_hostgroup;srv_template
```

RUN
----
```
ansible-playbook import-hosts.yml
```

