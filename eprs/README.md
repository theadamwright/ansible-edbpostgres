# EDB Postgres Prerequisites

Playbooks in the EPRS directory will: 

* Install EPRS7  
* Start EPRS7 as a system service 
* Tune EPRS7 with best practice configurations
* Add Leader Node to the Replication Server 
* Set the Database Password 
* Set the Replication Server Admin password 
 
## Getting Started

### Installing

**edb-rs-01.install_eprs7.yml** does a basic installed of EPRS7. No Kafka or Zookeeper server is running at this point. 

```
ansible-playbook edb-rs-01.install_eprs7.yml --extra-vars "host=gcp-eprs"
```

**edb-rs-02.config_eprs7.yml** configure EPRS7 with EDB Best Practices and optionally places Kafa data on a seperate disk volume. 

```
ansible-playbook edb-rs-02.config_eprs7.yml --extra-vars "host=gcp-eprs rsdata=/opt/edb/rsdata"
```

**rs-03.start_eprs7.yml** Starts EPRS7 as a system service. 
```
ansible-playbook edb-rs-03.start_eprs7.yml --extra-vars "host=gcp-eprs"
```

**rs-03.start_eprs7.yml** Starts EPRS7 as a system service. 
```
ansible-playbook edb-rs-03.start_eprs7.yml --extra-vars "host=gcp-eprs"
```
**edb-rs-04.set_passwords_eprs7.yml** 
One time playbook to set the replication admins password and database credentials to be encrypted for use with the repcli. 
```
ansible-playbook edb-rs-04.set_passwords_eprs7.yml --extra-vars "host=ansible-tests dbpwd=3db_pwd rspwd=adm1n"
```

**edb-rs-05.join_network_eprs7.yml**

Add the nodes to the Replication (Kafka) network 
```
ansible-playbook edb-rs-05.join_network_eprs7.yml --extra-vars "host=gcp-eprs"
```

**edb-rs.stop_eprs7.yml** playbook is used for ad-hoc stopping of the EPRS7 system service. 
```
ansible-playbook edb-rs.stop_eprs7.yml --extra-vars "host=gcp-eprs"
```