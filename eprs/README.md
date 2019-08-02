# EDB Postgres Prerequisites

Playbooks in the EPRS directory will: 

* Install EPRS7  
* Start EPRS7 as a system service 
* Tune EPRS7 with best practice configurations
* Add Leader Node to the Replication Server 
* Set the Replication Server Admin password 
* Add additional nodes to the EPRS cluster
 
## Prerequisite

EDB Repo should be installed and configured before running the EPRS playbooks. 

If the EDB repo is not already installed and configured, refer to `../prereqs/README.md`

## Getting Started

### Playbooks

**edb-rs-01.install_eprs7.yml** does a basic installation of EPRS7. No Kafka or Zookeeper server is running at this point. This playbook should be run on all EPRS servers. 

```
ansible-playbook edb-rs-01.install_eprs7.yml --extra-vars "host=eprs-servers"
```

**edb-rs-02.config_eprs7.yml** configure EPRS7 with EDB Best Practices and optionally places Kafa data on a seperate disk volume with the variable `rsdata`, omit variable to keep the default Kakfa data drive underneath `/var/lib/edb/rs`. This playbook should be run on all EPRS servers.  

```
ansible-playbook edb-rs-02.config_eprs7.yml --extra-vars "host=eprs-servers rsdata=/opt/edb/rsdata"
```

**rs-03.start_eprs7.yml** Starts EPRS7 as a system service. This playbook should be run on all EPRS servers.
```
ansible-playbook edb-rs-03.start_eprs7.yml --extra-vars "host=eprs-servers"
```

**edb-rs-04.init_leader_eprs7.yml** One time playbook to set the replication admin password and start the leader node. Add the host variable for the Leader node and the admin password user the `rspwd` variable. 

Note: This playbook should only be run on the leader node during initial deployment of this EPRS cluster.
```
ansible-playbook edb-rs-04.set_passwords_eprs7.yml --extra-vars "host=eprs1 rspwd=adm1n"
```

**edb-rs-05.join_network_eprs7.yml** Add additional EPRS nodes to the EPRS cluster. This playbook should be run for each non leader node joining the EPRS cluster. The `repcli -joinnetwork` command requires a logical name for the node and the real IP on the joining node. These are passed in as variables `joiningname` and `joiningip`. 

```
ansible-playbook edb-rs-05.join_network_eprs7.yml --extra-vars "host=eprs2 joiningname=eprs2 joiningip=192.168.1.2"

ansible-playbook edb-rs-05.join_network_eprs7.yml --extra-vars "host=eprs3 joiningname=eprs3 joiningip=192.168.1.3"
```

**edb-rs.stop_eprs7.yml** playbook used for ad-hoc stopping of the EPRS7 system service. 
```
ansible-playbook edb-rs.stop_eprs7.yml --extra-vars "host=gcp-eprs"
```