# Ansible role: labocbz.install_lynis

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Git](https://img.shields.io/badge/Tech-Git-orange)
![Tag: Cron](https://img.shields.io/badge/Tech-Cron-orange)
![Tag: Mailutils](https://img.shields.io/badge/Tech-Mailutils-orange)
![Tag: Colorized-logs](https://img.shields.io/badge/Tech-Colorized--logs-orange)
![Tag: Kbtin](https://img.shields.io/badge/Tech-Kbtin-orange)
![Tag: Lynis](https://img.shields.io/badge/Tech-Lynis-orange)

An Ansible role to install and configure Lynis on your host.

The Ansible role installs Lynis, a system analysis tool, on the target system. The role utilizes Git to clone the Lynis repository, ensuring that updates are available. Administrators may need to rerun the role periodically to obtain the latest updates (an enhancement could be implemented using a cron job).

Additionally, the role provides the ability to define a cron job for running system analyses using Lynis. This cron job can be customized by specifying the desired weekday, minute, and hour for execution. The analysis results are then sent via email to the specified address.

Please note that the presence of an email server is required for sending the analysis reports. The role does not install an email server but relies on an existing one to handle the report delivery.

By deploying Lynis with this role, administrators can conduct routine system analyses, benefiting from the insights and security recommendations provided by the tool. The option to schedule these analyses via a cron job allows for automated and regular assessments, ensuring continuous monitoring and improvement of system security.

In summary, the Lynis role offers a comprehensive solution for system analysis, incorporating regular updates, automated cron job scheduling, and analysis result email delivery. By leveraging this role, administrators can enhance their system's security posture and stay informed about potential vulnerabilities and security improvements.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_lynis__create_cron_job: true
install_lynis__cron_job_weekday: "*"
install_lynis__cron_job_minute: "6"
install_lynis__cron_job_hour: "6"

install_lynis__report_email_address: "your.address@domain.tld"

install_lynis__install_path: "/usr/local/lynis"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_lynis__create_cron_job: true
inv_install_lynis__cron_job_weekday: "*"
inv_install_lynis__cron_job_minute: "6"
inv_install_lynis__cron_job_hour: "6"

inv_install_lynis__report_email_address: "your.address@domain.tld"

inv_install_lynis__install_path: "/usr/local/lynis"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_lynis"
    tags:
    - "labocbz.install_lynis"
    vars:
    install_lynis__create_cron_job: "{{ inv_install_lynis__create_cron_job }}"
    install_lynis__cron_job_weekday: "{{ inv_install_lynis__create_cron_job }}"
    install_lynis__cron_job_minute: "{{ inv_install_lynis__cron_job_minute }}"
    install_lynis__cron_job_hour: "{{ inv_install_lynis__cron_job_hour }}"
    install_lynis__report_email_address: "{{ inv_install_lynis__create_cron_job }}"
    install_lynis__install_path: "{{ inv_install_lynis__install_path }}"
    ansible.builtin.include_role:
    name: "labocbz.install_lynis"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2024-02-24: Fix and CI

* Added support for new CI base
* Edit all vars with __

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
