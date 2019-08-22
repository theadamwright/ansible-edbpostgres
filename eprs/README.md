# EDB Postgres Prerequisites

Playbooks in the EPRS directory will: 

* Install EPRS7  
* Start EPRS7 as a system service 
* Tune EPRS7 with best practice configurations
* Add Leader Node to the Replication Server 
* Set the Replication Server Admin password 
* Add additional nodes to the EPRS cluster
 
## Prerequisite

EDB Repo should already be installed and configured before running the EPRS playbooks. 

If the EDB repo is not already installed and configured, refer to `../prereqs/README.md`

## Getting Started

### Playbooks

**edb-rs-01.install_eprs7.yml** does a basic installation of EPRS7. No Kafka or Zookeeper server is running at this point. This playbook runs on all EPRS servers. 

```
ansible-playbook eprs/edb-rs-01.install_eprs7.yml -i inventory.ini
```

**edb-rs-02.config_eprs7.yml** configures EPRS7 with EDB Best Practices and optionally places Kafa data on a seperate disk volume with the variable `rsdata`, omit variable to keep the default Kakfa data drive underneath `/var/lib/edb/rs`. This playbook runs on all EPRS servers.  

```
ansible-playbook eprs/edb-rs-02.config_eprs7.yml  -i inventory.ini --extra-vars "rsdata=/opt/edb/rsdata"
```

**rs-03.start_eprs7.yml** Starts EPRS7 as a system service. This playbook is run on all EPRS servers.
```
ansible-playbook eprs/edb-rs-03.start_eprs7.yml -i inventory.ini 
```

**edb-rs-04.init_leader_eprs7.yml** One time playbook to set the replication admin password and start the leader node. Add the host variable for the Leader node and the admin password user the `rspwd` variable. 

Note: This playbook should only be run on the leader node during initial deployment of this EPRS cluster.
```
ansible-playbook eprs/edb-rs-04.init_leader_eprs7.yml  -i inventory.ini --extra-vars "rspwd=adm1n"
```

**edb-rs-05.join_network_eprs7.yml** Add additional EPRS nodes to the EPRS cluster. This playbook is run for each non leader node joining the EPRS cluster. The `repcli -joinnetwork` command requires a logical name for the node and the real IP on the joining node. These are passed in as variables `joiningname` and `joiningip`. 

```
ansible-playbook edb-rs-05.join_network_eprs7.yml -i inventory.ini --extra-vars "joiningname=eprs2 joiningip=192.168.1.2"

ansible-playbook edb-rs-05.join_network_eprs7.yml -i inventory.ini --extra-vars "joiningname=eprs3 joiningip=192.168.1.3"
```

**edb-rs.stop_eprs7.yml** playbook used for ad-hoc stopping of the EPRS7 system service. 
```
ansible-playbook edb-rs.stop_eprs7.yml --i inventory.ini --extra-vars "host=eprs1"
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