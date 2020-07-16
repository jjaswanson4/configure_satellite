# configure_satellite

Ansible collection to configure satellite.

This collection is also available on [ansible galaxy](https://galaxy.ansible.com/jjaswanson4/configure_satellite).

## Overview
This collection will configure satellite via the [foreman ansible modules](https://theforeman.org/plugins/foreman-ansible-modules).

## Usage
The best way to consume this collection is to set up a requirements.yml:
```yaml
---
collections:
  - jjaswanson4.configure_satellite
```
The collection can also be installed from the command line ad-hoc:
```
ansible-galaxy collection install jjaswanson4.configure_satellite
```

## Examples

## Vars
All vars are defined in a dictionary stored in a vars file included at the playbook level. There are two roles that configure satellite: configure_foreman and configure_katello. Foreman components (such as compute resources, subnets, etc) are defined under satellite.foreman, and katello settings (such are content views, repositories, etc) are defined under satellite.katello:
```yaml
satellite:
  foreman:
	katello:
```

- Organizations: list of satellite organizations to be created, multiple are supported. At least one must be defined with `initial_organization` set to `true`.
```yaml
satellite:
  foreman:
	  organizations:
		  - name: org_1
			  initial_organization: true
		  - name: org_2
			- name: org_3
```
			  
