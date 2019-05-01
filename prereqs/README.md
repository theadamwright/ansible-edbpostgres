# EDB Postgres Prerequisites

The playbooks in the prereqs directory need to be played before installing EDB Postgres software. This will setup the EDB YUM Repo, allowing for installation of EDB software using Yaast Update Manager.  

## Getting Started



### Prerequisites

1. The playbooks assume a internet connection to yum.enterprisedb.com.

2. A EDB Repo account is required to install software from yum.enterprisedb.com. Access to EDB Software Repositories can be requested here: https://www.enterprisedb.com/repository-access-request

3. EDB YUM credentials should be set as an environment variable on the Ansible controller server. To set once: 

```
export EDBYUMUSERNAME="edb-adamwright"
export EDBYUMPASSWORD="00001111222233334444"
```

### Installing

Playbook **1-edb-prereqs.edb_repo_install.yml** installs the edb repo file onto each guest. After installing, the guest will have a edb.repo file in /yum/repos.d/ .

In the example for **1-edb-prereqs.edb_repo_install.yml** , we have a group of edb-servers in our Ansible hosts file that will all have the EDB repo installed:
```
ansible-playbook 1-edb-prereqs.edb_repo_install.yml --extra-vars "host=edb-servers"
```

Playbook **2-edb-prereqs.edb_repo_configure.yml** configures the edb.repo file on guest machines with YUM username and password credentials read from environment variables set on the Ansible Controller. 

In this example for **2-edb-prereqs.edb_repo_configure.yml** , we have a group of edb-servers in our Ansible hosts file that will  have the yumusername and yumpassword updated with the credentials read from the Ansible Controller. 
```
ansible-playbook 2-edb-prereqs.edb_repo_configure.yml --extra-vars "host=edb-servers"
```

## Authors

Adam Wright 

## License



