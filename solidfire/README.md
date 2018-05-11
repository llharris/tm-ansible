# Ansible Roles for SolidFire Storage Configuration

Directory layout is as follows...
```
.
├── ansible.cfg
├── group_vars
│   ├── sf_nodes
│   └── solidfire
│       ├── vars
│       └── vault
├── hosts
├── host_vars
│   ├── sf-node1
│   │   ├── vars
│   │   └── vault
│   ├── sf-node2
│   │   ├── vars
│   │   └── vault
│   ├── sf-node3
│   │   ├── vars
│   │   └── vault
│   ├── sf-node4
│   │   ├── vars
│   │   └── vault
│   └── sf-node5
│       ├── vars
│       └── vault
├── README.md
├── requirements.txt
├── roles
│   └── role.name
│       ├── defaults
│       │   └── main.yml
│       ├── files
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

## Ansible.cfg

Slightly modified ansible.cfg from default. Disabled host key checking and the creation of .retry files.

## Group_Vars / Host_Vars

Setup uses a `vars` file for each group and host which contains all variables. Sensitive variables are abstracted again into a `vault` file, again one per group and/or host.
For example, group_vars/solidfire/vars contains sf_username variable which references a secret value in the vault file like so: `sf_username: '{{ vault_sf_username }}'`

## Hosts

Not a hosts file in the traditional sense, really only used for variables as all of the work is done through a 'local' connection as we're either making Python SDK calls or using the Ansible URI module so localhost is just acting as a REST client.

## requirements.txt

Use this to install required python modules by running `pip install -r requirements.txt`

## Roles

There's a subdirectory for each role. Each role is defined as a standalone operation, e.g. create a user, create a volume. A role might contain multiple tasks, but they'll all be focussed on achieving one operation.

## site.yml

This playbook can be used to call all of the roles in a specifically defined order.

## Vault Passwords

Vault passwords can be passed in at runtime: `ansible-playbook -i hosts site.yml --ask-vault-pass` or by referencing a file which contains the vault password: `ansible-playbook -i hosts site.yml --vault-password-file=myvaultpass.txt`. The file containing the vault password is obviously excluded from this repo.

