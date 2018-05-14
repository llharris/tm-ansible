# Ansible Roles for SolidFire Storage Configuration

Directory layout is as follows...
```
.
└── solidfire
    ├── ansible.cfg
    ├── group_vars
    │   └── solidfire
    │       ├── vars
    │       └── vault
    ├── hosts
    ├── README.md
    ├── requirements.txt
    ├── roles
    │   ├── sf.con_check
    │   │   ├── defaults
    │   │   │   └── main.yml
    │   │   ├── files
    │   │   ├── handlers
    │   │   │   └── main.yml
    │   │   ├── meta
    │   │   │   └── main.yml
    │   │   ├── README.md
    │   │   ├── tasks
    │   │   │   └── main.yml
    │   │   ├── templates
    │   │   ├── tests
    │   │   │   ├── inventory
    │   │   │   └── test.yml
    │   │   └── vars
    │   │       └── main.yml
    │   └── sf.ntp_config
    │       ├── defaults
    │       │   └── main.yml
    │       ├── files
    │       │   └── ntp.json
    │       ├── handlers
    │       │   └── main.yml
    │       ├── meta
    │       │   └── main.yml
    │       ├── README.md
    │       ├── tasks
    │       │   └── main.yml
    │       ├── templates
    │       ├── tests
    │       │   ├── inventory
    │       │   └── test.yml
    │       └── vars
    │           └── main.yml
    └── site.yml

```
## Supported Element OS Versions

These playbooks have been tested against Element OS 10.1.

## Basic Operation

There are only a small number of SolidFire Ansible modules available, so most interactions use the Ansible URI module which allows us to interact directly with the REST API by POSTing a JSON body. The 

## Ansible.cfg

Slightly modified ansible.cfg from default. Disabled host key checking and the creation of .retry files, turned on logging.

## Group_Vars

group_vars are just used for the initial connection parameters needed to be able to talk to the SolidFire REST API. These include the IP, REST URL, username and password. Sensitive variables are abstracted again into a `vault` file. For example, group_vars/solidfire/vars contains sf_username variable which references a secret value in the vault file like so: `sf_username: '{{ vault_sf_username }}'` You will need to change the `sf_hostname` variable to use these playbooks against different SolidFire systems. 

## Hosts

The inventory file `hosts` just contains a localhost definition for the `[solidfire]` group. As we're interacting with the REST API primarily, everything is being executed on localhost rather than connecting over SSH to perform tasks like we'd expect with a Linux server for example. The inventory file should not need to be changed.

## requirements.txt

Use this to install required python modules by running `pip install -r requirements.txt`

## Roles

There's a subdirectory for each role. Each role is defined as a standalone operation, e.g. create a user, create a volume. A role might contain multiple tasks, but they'll all be focussed on achieving one operation.

## site.yml

This playbook can be used to call all of the roles in a specifically defined order.

## Vault Passwords

Vault passwords can be passed in at runtime: `ansible-playbook -i hosts site.yml --ask-vault-pass` or by referencing a file which contains the vault password: `ansible-playbook -i hosts site.yml --vault-password-file=myvaultpass.txt`. The file containing the vault password is obviously excluded from this repo.

