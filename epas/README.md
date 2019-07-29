# EDB Postgres Prerequisites

Playbooks in the EPAS directory will: 

* Install EPAS 10 and up 
* Initialize a new EPAS database cluster with a custom PGDATA and PGWAL directory
* Configure EPAS with EDB Best Practices for performance and durability 
* Manage EPAS configuration files 
 

## Getting Started

### Prerequisites

EDB repo should be installed and configured with a yumusername and yumpassword. 


### Installing

**edb-as-01.install_epas.yml** performs a basic installation of EPAS. Please note, with the RPM installation, a database is not automatically intialized. That is done in a seperate playbook. 

Note: If the `pgmajor` variable is not supplied, the playbook will default to installing *EPAS 11*
```
ansible-playbook edb-as-01.install_epas.yml --extra-vars "host=epas-servers pgmajor=11"
```

**edb-as-02.initialize_epas** will initialize a new EPAS cluster and configure the system service to start on boot 

Custom pgdata and pgwal are optional: 
```
ansible-playbook edb-as-02.initialize_epas.yml --extra-vars "host=gpc-dbservers pgdata=/opt/edb/pgdata pgwal=/opt/edb/pgwal pgmajor=11"
```
**edb-as-03.config_epas.yml** configures EPAS with EDB Best Practices configurations. 

CIDR options for pg_hba.conf are optional 
```
ansible-playbook edb-as-03.config_epas11.yml --extra-vars "host=gpc-dbservers pgmajor=11 cidr1=10.142.0.0/24 cidr2=10.138.0.0/24"
 ```

**edb-as-restart-epas.yml** is a playbook to be used when ad-hoc restarts of the database servers are required. 
```
ansible-playbook edb-as-restart-epas.yml --extra-vars "host=gpc-dbservers pgmajor=11"
```
**edb-as.config_epas-for-logical-replication.yml** configures the database server with database prameters required for logical replication. 
```
ansible-playbook edb-as.config_epas-for-logical-replication.yml --extra-vars "host=gpc-dbservers pgmajor=11"
```