# Uptane/OTA Community Edition Demo Server

## Overview

This repo contains infrastructure files and scripts to set up the demo instance of OTA Community Edition available at https://uptanedemo.org.

This demo server is publicly accessible, and can be used by anyone by configuring [aktualizr](https://github.com/uptane/aktualizr) to connect to it. However, it will wipe itself (including all user data and keys) every day at 3AM UTC. No web UI is provided, so all interaction with the server can only take place via API.

## Ansible roles

- `hard-wipe-demo-server`: Wipe and reboot server into a fresh linux install. Take approximately 8-10 minutes to complete
- `configure-security`: Configure UFW and SSH to standard, semi-hardened configurations
- `install-prereqs`: Install the necessary packages for running the demo server
- `configure-demo-server`: Clone OTA Community Edition and modify base config files as necessary for demo server
- `generate-keys`: Generate Uptane key material and certificate authority for device certs
- `start-server`: Start OTA Community Edition (docker-compose) and reverse proxy (TBD)
- `soft-wipe-demo-server`: Stop services, then delete all key material and user data

## Playbooks

* `setup-server` contains:
  * `hard-wipe-demo-server`
  * `configure-security`
  * `install-prereqs`
  * `configure-demo-server`
* `soft-reset` contains:
  * `soft-wipe-demo-server`
  * `generate-keys`
  * `start-server`
* `hard-reset` contains:
  * `hard-wipe-demo-server`
  * `configure-security`
  * `install-prereqs`
  * `configure-demo-server`
  * `generate-keys`
  * `start-server`
  * `soft-wipe-demo-server`

## CI Automation

This repository periodically resets the server via github actions. Once per day at 3AM UTC, it will run the `soft-reset` playbook. Once every two weeks, it will run the `hard-reset` playbook.