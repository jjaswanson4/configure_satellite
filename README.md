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

- Organizations: list of satellite organizations, at least one must be defined with `initial_organization` set to `true`.
```yaml
satellite:
  foreman:
    organizations:
      - name: org_1
        initial_organization: true
      - name: org_2
      - name: org_3
```
- Locations: list of satellite locations, at least one must be defined with `initial_location` set to `true`. Locations can be mapped to 1 or more organization.
```yaml
satellite:
  foreman:
    locations:
      - name: loc_1
        initial_location: true
        organizations:
          - name: org_1
          - name: org_2
          - name: org_3
      - name: loc_2
        organizations:
          - name: org_2
      - name: loc_3
        organizations:
          - name: org_2
          - name: org_3
```
- domains: list of domains mapped to one or more organizations and locations.
```yaml
satellite:
  foreman:
    domains:
      - name: domain1.internal.lcl
        description: default dns domain
        organizations:
          - name: org_1
          - name: org_3
        locations:
          - name: loc_1
          - name: loc_3
      - name: domain2.internal.lcl
        description: secondary dns domain
        organizations:
          - name: org_2
        locations:
          - name: loc_2
```
- subnets: list of subnets mapped to one or more organizations, locations, and domains.
```yaml
satellite:
  foreman:
    subnets:
      - name: test-subnet-192.168.0.0_24
        network: 192.168.0.0
        mask: 255.255.255.0
        gateway: 192.168.0.1
        dns_primary: 192.168.0.10
        dns_secondary: 192.168.1.11
        domains:
          - name: domain1.internal.lcl
        organizations:
          - name: org_1
          - name: org_3
        locations:
          - name: loc_1
          - name: loc_3
      - name: prod-subnet-10.1.0.0_22
        network: 10.1.0.0
        mask: 255.255.252.0
        gateway: 10.1.0.1
        dns_primary: 10.1.0.10
        dns_secondary: 10.1.0.11
        domains:
          - name: domain2.internal.lcl
        organizations:
          - name: org_2
        locations:
          - name: loc_2
```
