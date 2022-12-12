[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![Linux](https://svgshare.com/i/Zhy.svg)](https://svgshare.com/i/Zhy.svg)
[![Ask Me Anything !](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)](https://GitHub.com/Naereen/ama)

ANSIBLE ROLE for GCP SECRET MANAGER
=========

Get secrets from GCP SECRET MANAGER and put it on specified files

Requirements
------------

- authentication OK to your GCP account on the right project
- Have your secrets on GCP Secret Manager
- Enable API:

> Secret Manager (secretmanager.googleapis.com)

```
$ gcloud services list --available | grep Secret
secretmanager.googleapis.com
$ gcloud services enable secretmanager.googleapis.com
```

Role Variables
--------------

```
# var "gcp_secrets" must be defined as dictionary

gcp_secrets:
  SECRET_NAME_1: # give it an arbitrary name, could be the same as 'name'
    name: SECRET_NAME_1 # as its exact name in GCP Secret Manager
    file_path: "/PATH/TO/TARGET_FILE/SECRET_NAME"
    file_owner: USER # maybe root is your secret keeper user
    file_group: USER # if not specified, default is "file_owner"
    file_mode: '0400' # be sure keep it safe
  SECRET_NAME_2:
    name: SECRET_NAME_2
    file_path: "/PATH/TO/TARGET_FILE/SECRET_NAME"
    file_owner: USER
    file_mode: '0644'
  SECRET_NAME_N:
    name: SECRET_NAME_N
    file_path: "/PATH/TO/TARGET_FILE/SECRET_NAME"
    file_owner: USER
    file_mode: '0600'
```

Dependencies
------------

None.

Example Playbook
----------------

```
- hosts: web-servers
  vars:
    gcp_secrets:
      database:
        name: MYSQL_PASSWORD
        file_path: "/srv/mysql_pwd"
        file_owner: root
        file_group: root
        file_mode: '0400'
      nexus:
        name: NEXUS_PASSWORD
        file_path: "/srv/nexus_pwd"
        file_owner: root
        file_mode: '0400'
  roles:
    - gcp_secret_manager
```

What to improve
------------

- [ ] Manage secrets: add, edit, delete
- [ ] Edit secret properties (version, replication, location, labels, iam)

License
-------

Apache

Author Information
------------------

Created by [Niaina Lens](https://github.com/niainaLens) \
September 2022
