---
layout: post
title:  "Ansible Notes"
date:   2017-01-11 22:22:03 -0500
categories: ansible
---

Notes I took while working with Ansible for the first time.

- terms ([_source_](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#.qfoew8ac2)):
  - `orchestration tools`: designed to provision the servers themselves, e.g. CloudFormation, Terraform
  - `configuration management tools`: designed to install and manage software on existing servers, e.g. Chef, Puppet, Ansible, and SaltStack
- Ansible is a **procedural configuration management tool**
- list hosts that will be targeted by the given playbook

    ```bash
    $ ansible-playbook pb.yml -i ../environments/dev --list-hosts
    ```
- verbose mode up to 3 levels

    ```bash
    $ ansible-playbook pb.yml -i ../environments -vvv
    ```
- `role_path` is relative the location of the `ansible.cfg` ansible config file
- [dry run](http://docs.ansible.com/ansible/playbooks_checkmode.html)

    ```bash
    $ ansible-playbook pb.yml --check
    ```
- [multistage environments](https://www.digitalocean.com/community/tutorials/how-to-manage-multistage-environments-with-ansible)

    ```
    ├── environments
    │   ├── cross_env_vars
    │   ├── dev
    │   │   ├── ec2.ini -> ../ec2.ini
    │   │   ├── ec2.py -> ../ec2.py
    │   │   └── group_vars
    │   │       ├── all
    │   │       │   └── cross_env_vars -> ../../../cross_env_vars
    │   │       └── tag_role_api
    │   │           └── vars
    │   │           └── vault
    │   ├── prod
    │   │   ├── ec2.ini -> ../ec2.ini
    │   │   ├── ec2.py -> ../ec2.py
    │   │   └── group_vars
    │   │       ├── all
    │   │       │   └── cross_env_vars -> ../../../cross_env_vars
    │   │       └── tag_role_api
    │   │           └── vars
    │   │           └── vault
    │   ├── ec2.ini
    │   └── ec2.py
    ```
- `with_items` [using dicts](http://docs.ansible.com/ansible/playbooks_loops.html#id13)
- `template` accepts vars (this is not documented clearly)

    ```
    - template: src=test.j2 dest=/tmp/File1
      vars:
        myTemplateVariable: myDirName

    - template: src=test.j2 dest=/tmp/File2
      vars:
        myTemplateVariable: myOtherDir
    ```
