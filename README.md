# Ansible Azure training provisioner
**azure_lab_setup** is an automated lab setup for Ansible training on Azure.

# Table Of Contents
- [Requirements](#requirements)
- [Lab Setup](#lab-setup)
  - [Setup (per workshop)](#setup-per-workshop)
  - [Accessing student documentation and slides](#Accessing-student-documentation-and-slides)
- [Lab Teardown](#azure-teardown)

# Requirements
- This provisioner must be run with Ansible Engine v2.8.0 or higher.
- Azure Account
- To create Azure login information follow: https://www.terraform.io/docs/providers/azurerm/auth/service_principal_client_secret.html

# Lab Setup

## Setup (per workshop)

1. Define the following variables in a file passed in using `-e @extra_vars.yml`

```
---
azure_region: eastus                   # region where the nodes will live
azure_name_prefix: TESTWORKSHOP        # name prefix for all the VMs
student_total: 2                       # creates student_total of workbenches for the workshop
domain_name:                           # to create the DNS names in Azure. For example the eastus region would be 'eastus.cloudapp.azure.com'
azure_subscription_id:
azure_client_id:
azure_client_secret:
azure_tenant_id:
admin_password: RedHat123!             # password for Ansible control node, defaults to ansible
towerinstall: true                     # automatically installs Tower to control node
autolicense: false                     # automatically licenses Tower if license is provided
```

If you want to license it you must copy a license called tower_license.json into this directory.  If you do not have a license already please request one using the [Workshop License Link](https://www.ansible.com/workshop-license).


2. Run the playbook:

        ansible-playbook provision_lab.yml -e @extra_vars.yml --ask-become-pass

        (The become-pass will be the admin_password in the extra_vars.yml)

3. Login to the Azure console and you should see instances being created like:

        `TESTWORKSHOP-student1-ansible`

## Accessing student documentation and slides

The workshop will be [Here](https://github.com/Joel-Adams/redhatgov.github.io) under the azure_infra_ansible workshop.

# Lab Teardown

The `teardown_lab.yml` playbook deletes all the training instances as well as local inventory files.

To destroy all the Azure instances after training is complete:

1. Run the playbook:

        ansible-playbook teardown_lab.yml -e @extra_vars.yml --ask-become-pass

# FAQ
For frequently asked questions see the [FAQ](../docs/faq.md)
