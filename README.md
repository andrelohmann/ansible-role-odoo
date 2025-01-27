# odoo

![Last test](https://github.com/andrelohmann/ansible-role-odoo/actions/workflows/test.yml/badge.svg)

## Content

Use this role to install the latest odoo version from source on your ubuntu server.

### Requirements

This role requires ubuntu

### Role Variables

Set the odoo variables and config parameters

    odoo_release: latest  # e.g. 17, 18
    # set odoo_source_install to false, to install from nightl packages
    odoo_source_install: false
    odoo_config: []
    # Use the key / value format for config parameters
    # - key: db_name
    #   value: odoo
    # - key: admin_passwd
    #   value: S3cr3t!
    # Preseed databases
    odoo_init_databases: []
    #- database: odoo_1
    #  language: de_DE

### Example Playbook

    - hosts: odoo
      become: yes
      roles:
      - { role: andrelohmann.odoo }

## Role Development

### Special purpose

This repository supports the following features for the role development:

* yamllint
* ansible-lint
* molecule test
* github action
* auto version-up
* update ansible-galaxy
* show build status
* test within vagrant (for development purpose)
* test with molecule (inside or outside vagrant)
* test against docker container
* test and develop inside vscode

### Prerequisites

https://thedatabaseme.de/2022/01/17/automated-testing-your-ansible-role-with-molecule-and-github-actions/

* Virtualbox + Vagrant installed (only necessary, if the role should be tested with vargant as well)
* Docker Desktop
* VisualStudioCode + remote extensionpack (dependencies are defined within .vscode/extensions.json)

### Development-Setup

This ansible role is developed using molecule for testing. It's development is based on visual studio code and a regarding development container, solving all dependencies in terms of necessary tools (ansible, linter, molecule).

The role will be tested on ubuntu noble.

To startup the molecule test containers from within the development container, the docker socket needs to be bind mounted into the development container.

#### Important folders and files

##### .devcontainer

* Defines the Dockerfile for the development container
* Configures the development container startup (e.g. bind mount docker socket)

##### molecule/default/Dockerfile.js

* Used as a template for all platforms defined in molecule/default/molecule.yml
* Prepares the environments to support systemd services (necessary for some ansible roles acting on systemd)
* Installs all requirements to run ansible against the derived container
* The file is aligned with the attributes of the platforms in molecule/default/molecule.yml
* For more information - study the molecule documentation

### Usage

#### Visual Studio Code

* Change to the root directory of your role and start vscode

```
code .
```

 * From within the development container you can use the following run commands

```
yamllint .
ansible-lint .
molecule create
molecule test --all
```

#### Vagrant + Virtualbox

* Change to the root directory of your role
* Change to the vagrant folder
* Start and enter the vagrant machine

```
vagrant up
vagrant ssh
```

* Change to the role folder

```
cd /etc/ansible/roles/ansible-role- [tab]
```

* Now you can run all tests

```
yamllint .
ansible-lint .
molecule create
molecule test --all
```

### Build and Release process

The ansible role defines a bunch of github workflows to run molecule test and release management.

The release management requires a handful of settings.

#### Protecting the master/main branch

* Settings -> Branches -> Add branch protection rule
* Branch pattern name -> main or master (depending on your default branch)
* Protect matching branches -> check "Require a pull request before merging"
* "Require approvals" can be individually handled as required

#### Give read and write permissions to GITHUB_TOKEN

* Settings -> Actions -> General -> Workflow permissions -> read and write permissions

#### Commit messages

Commit messages should follow a special format to achieve a patch, minor or major semantic versioning update.

##### patch

0.0.x

```
fix(single_word): description
```

##### minor

0.x.0

```
feat(single_word): description
```

##### major

x.0.0

```
perf(single_word): description
BREAKING CHANGE: describing the breaking change
```

It's absolutely important, that "BREAKING CHANGE: " is mentioned in the second+ line. On single line commit messages, the major version update will be ignored.

#### Add GALAXY_API_KEY secret

* Authenticate yourself with your github account at https://galaxy.ansible.com/.
* Fetch galaxy api key from Preferences -> API Key
* Open your github role repository
* Settings -> Secrets and variables -> Actions -> New repository secret
* Use "GALAXY_API_KEY" as key and the copied galaxy API key as value

## License

MIT

## Author Information

&copy; Andre Lohmann (and others) 2024

https://github.com/andrelohmann

### Maintainer Contact

* Andre Lohmann
  <lohmann.andre (at) gmail (dot) com>
