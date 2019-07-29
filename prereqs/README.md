# EDB Postgres Prerequisites

The playbook in the prereqs directory installs the EDB YUM repo on host server and configures the edb.repo file with the YUM username and YUM Password pulled from the following environment variables on the Ansible Controller: `EDBYUMUSERNAME` and `EDBYUMPASSWORD`

The playbook in the pre-reqs directory need to be played before installing EDB Postgres software using th. The playbook requires `EDBYUMUSERNAME` and `EDBYUMPASSWORD` to be set on the Ansible Controller. 

APT coming soon...

## Getting Started


### Prerequisites

1. The playbooks assume a internet connection to yum.enterprisedb.com.

2. A EDB Repo account is required to install software from yum.enterprisedb.com. Access to EDB Software Repositories can be requested here: https://www.enterprisedb.com/repository-access-request

3. EDB YUM credentials should be set as an environment variable on the Ansible controller server. To set once: 

```
export EDBYUMUSERNAME="awright"
export EDBYUMPASSWORD="00001111222233334444"
```

### Installing

Playbook **1-edb-prereqs.edb_repo_config.yml** installs the `edb.repo` file onto each host. After installing, the hosts will have a `edb.repo` file in `/yum/repos.d/`. The YUM username and password credentials read from environment variables set on the Ansible Controller are then updated on the  `edb.repo` file.   

In the example for **1-edb-prereqs.edb_repo_config.yml**, there is a global group edb-servers in our Ansible hosts file that will all have the EDB repo installed:

```
ansible-playbook 1-edb-prereqs.edb_repo_config.yml --extra-vars "host=edb-servers"
```

## Authors

Adam Wright 

## License