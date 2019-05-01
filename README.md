# Ansible Azure training provisioner
**azure_lab_setup** is an automated lab setup for Ansible training on Azure. 

# Table Of Contents
- [Requirements](#requirements)
- [Lab Setup](#lab-setup)
  - [One Time Setup](#one-time-setup)
  - [Setup (per workshop)](#setup-per-workshop)
  - [Accessing student documentation and slides](#Accessing-student-documentation-and-slides)
- [Lab Teardown](#azure-teardown)
- [FAQ](../docs/faq.md)

# Requirements
- This provisioner must be run with Ansible Engine v2.7.0 or higher.
- Azure Account (follow directions on one time setup below)
- To create Azure login information follow: https://www.terraform.io/docs/providers/azurerm/auth/service_principal_client_secret.html

# Lab Setup
[For One Time Setup - click here](../docs/setup.md)

## Setup (per workshop)

1. Define the following variables in a file passed in using `-e @extra_vars.yml`

```
---
azure_region: eastus                   # region where the nodes will live
azure_name_prefix: TESTWORKSHOP        # name prefix for all the VMs
student_total: 2                       # creates student_total of workbenches for the workshop
azure_subscription_id:
azure_client_id:
azure_client_secret:
azure_tenant_id:
#OPTIONAL VARIABLES
admin_password: RedHat123!             # password for Ansible control node, defaults to ansible
networking: true                       # Set this if you want the workshop in networking mode
create_login_page: true                # creates Azure website for azure_name_prefix.workshop_dns_zone
workshop_dns_zone: rhdemo.io           # Sets the DNS zone to use for the website
towerinstall: true                     # automatically installs Tower to control node
#autolicense: true                     # automatically licenses Tower if license is provided
#xrdp: true                            # install xrdp with xfce for graphical interface
```

If you want to license it you must copy a license called tower_license.json into this directory.  If you do not have a license already please request one using the [Workshop License Link](https://www.ansible.com/workshop-license).

For more extra_vars examples, look at the following:
- [sample-vars.yml](sample_workshops/sample-vars.yml) - example for the Ansible Engine Workshop
- [sample-vars-networking.yml](sample_workshops/sample-vars-networking.yml) - example for the **Ansible Network Workshop**
- [sample-vars-f5.yml](sample_workshops/sample-vars-f5.yml) - example for **Ansible F5 Workshop**
- [sample-vars-auto.yml](sample_workshops/sample-vars-auto.yml) - example for Tower installation and licensing

2. Run the playbook:

        ansible-playbook provision_lab.yml -e @extra_vars.yml --ask-become-pass

        (The become-pass will be the admin_password in the extra_vars.yml)

3. Login to the Azure console and you should see instances being created like:

        `TESTWORKSHOP-student1-ansible`

## Accessing student documentation and slides

  - Exercises and instructor slides are hosted at http://ansible.com/linklight
  - Workbench information is stored in two places after you provision:
    - in a local directory named after the workshop (e.g. TESTWORKSHOP/instructor_inventory)
    - if `create_login_page: true` is enabled, there will be a website ec2_name_prefix.workshop_dns_zone (e.g. TESTWORKSHOP.rhdemo.io)

# Lab Teardown

The `teardown_lab.yml` playbook deletes all the training instances as well as local inventory files.

To destroy all the EC2 instances after training is complete:

1. Run the playbook:

        ansible-playbook teardown_lab.yml -e @extra_vars.yml

# FAQ
For frequently asked questions see the [FAQ](../docs/faq.md)
