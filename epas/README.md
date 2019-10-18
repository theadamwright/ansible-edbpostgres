# EDB Postgres Advanced Server

Playbooks in the EPAS directory will: 

* Install EPAS 10 and up 
* Initialize a new EPAS database cluster with a custom PGDATA and PGWAL directory
* Configure EPAS with EDB Best Practices for performance and durability 
* Manage EPAS configuration files 
 

## Getting Started

### Prerequisites

EDB Repo should already be installed and configured before running the EPAS playbooks. 

If the EDB repo is not already installed and configured, refer to `../prereqs/README.md`


### Installing

**edb-as-01.install_epas.yml** performs a basic installation of EPAS. Please note, with the RPM installation, a database is not automatically intialized. That is done in a seperate playbook. 

Note: If the `pgmajor` variable is not supplied, the playbook will default to installing *EPAS 11*
```
ansible-playbook epas/edb-as-01.install_epas.yml -i inventory.ini --extra-vars "pgmajor=11"
```

**edb-as-02.initialize_epas** will initialize a new EPAS cluster and configure the system service to start on boot 

Custom pgdata and pgwal are optional: 
```
ansible-playbook epas/edb-as-02.initialize_epas.yml -i inventory.ini --extra-vars "pgdata=/opt/edb/pgdata pgwal=/opt/edb/pgwal pgmajor=11"
```
**edb-as-03.config_epas.yml** configures EPAS with EDB Best Practices configurations. 

CIDR options for pg_hba.conf are optional 
```
ansible-playbook epas/edb-as-03.config_epas.yml -i inventory.ini --extra-vars "pgmajor=11 cidr1=192.168.1.0/24 cidr2=192.168.2.0/24"
 ```

**edb-as-restart-epas.yml** is a playbook to be used when ad-hoc restarts of the database servers are required. Affected hosts are passed are defined when running thep laybook with the `host=` variable. 
```
ansible-playbook epas/edb-as-restart-epas.yml -i inventory.ini --extra-vars "host=epas1 pgmajor=11" 
```
**edb-as.config_epas-for-logical-replication.yml** configures the database server with database prameters required for logical replication. 
```
ansible-playbook epas/edb-as.config_epas-for-logical-replication.yml -i inventory.ini --extra-vars "host=epas1 pgmajor=11" 
```

## Authors

  Author: Adam Wright 
  
  E-mail ID: theadamwright@outlook.com

## License

 Copyright EnterpriseDB Corporation
 All rights reserved.
 Redistribution and use in source and binary forms, with or without
 modification, are permitted provided that the following conditions are
 met:
    * Redistributions of source code must retain the above copyright
      notice, this list of conditions and the following disclaimer.
    * Redistributions in binary form must reproduce the above copyright
      notice, this list of conditions and the following disclaimer in
      the documentation and/or other materials provided with the
      distribution.
    * Neither the name of PostgreSQL nor the names of its contributors
      may be used to endorse or promote products derived from this
      software without specific prior written permission.
 
 THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
 LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
 FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
 COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
 INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
 BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
 LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
 CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
 LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
 ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
 POSSIBILITY OF SUCH DAMAGE.
