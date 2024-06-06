# Ansible Playbook for Installing AWS Discovery Service Agent
This Ansible playbook automates the installation of the AWS Discovery Service Agent on Windows and Linux servers. It includes OS version detection, agent installation, and centralized logging of success or failure.

Documentation Reference: 
AWS Application Discovery Service:
https://docs.aws.amazon.com/application-discovery/latest/userguide/what-is-appdiscovery.html
Migration Hub Strategy Recommendations
https://docs.aws.amazon.com/migrationhub-strategy/latest/userguide/getting-started-dowmload-collector.html

## Requirements
Ansible installed on the control node.
Windows and Linux servers accessible via SSH (for Linux) and WinRM (for Windows).
AWS Discovery Service Agent installation URLs:
Windows: AWSDiscoveryAgentInstaller.exe
Linux: aws-discovery-agent.tar.gz

## Inventory File (inventory.yml)
The inventory file defines the servers on which the AWS Discovery Service Agent should be installed. It also includes variables for the AWS region, access key ID, and secret access key.

Define Windows and Linux servers on Inventory File
```yaml
all:
  vars:
    aws_region: "your-home-region"
    aws_access_key_id: "aws-access-key-id"
    aws_secret_access_key: "aws-secret-access-key"
    ansible_connection: "winrm"
    ansible_winrm_transport: "ntlm"
    ansible_winrm_server_cert_validation: "ignore"
  children:
    windows:
      vars:
        ansible_user: "Admin"
        ansible_password: "windows_password"
      hosts:
        - windows-server-1
        - windows-server-2
    linux:
      vars:
        ansible_user: "root"
        ansible_password: "linux_password"
      hosts:
        - linux-server-rhel1
        - linux-server-rhel2
        - linux-server-rhel3
```

## Ansible Playbook (install_aws_discovery_agent.yml)
The playbook performs the following tasks:

Checks if the AWS Discovery Agent is already installed.
Downloads and installs the agent on valid OS versions.
Starts the AWS Discovery Agent service.


To run the playbook:
```bash
ansible-playbook -i inventory.yml install_aws_discovery_agent.yml
```
To run a dry run:
```bash
ansible-playbook -i inventory.yml install_aws_discovery_agent.yml --check
```
## Syntax Test outputs
### Inventory
Output:
```bash
❯$ ansible-inventory --list -i inventory.yml
{
    "_meta": {
        "hostvars": {}
    },
    "all": {
        "children": [
            "ungrouped",
            "windows",
            "linux"
        ]
    }
}
```
### Playbook:
Output:
``` bash
❯$ ansible-playbook  ADS.yml  --syntax-check
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not
match 'all'
playbook: ADS.yml
```