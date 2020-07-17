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
All vars are defined in a dictionary stored in a vars file included at the playbook level. There are two roles that configure satellite: configure_foreman and configure_katello. Foreman components (such as compute resources, subnets, etc) are defined under satellite.foreman, and katello settings (such are content views, repositories, etc) are defined under satellite.katello. Below are two tables explaining variables and examples.

`satellite.foreman:`
| Name                     | Description
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| `organizations`          | List of satellite organizations, at least one must be defined with `initial_organization` set to `true`.                                               |
| `locations`              | List of satellite locations, at least one must be defined with `initial_location` set to `true`. Locations can be mapped to zero to many organization. |
| `domains`                | List of domains mapped to zero or many organizations and locations.                                                                                    |
| `subnets`                | List of subnets mapped to zero or many organizations, locations, and domains.                                                                          |
| `compute_resources`      | List of compute resources mapped to zaro or many organizations and locations.                                                                          |
| `compute_profiles`       | List of compute profiles mapped to a compute resource.                                                                                                 |
| `provisioning_templates` | List of provisioning templates mapped to zero to many organizations and locations. Expects a .erb file to exist on the server running ansible.         |
| `partition_tables`       | List of partition tables mapped to zero to many organizations and locations. Expects a .erb file to exist on the server running ansible.               |
| `settings`               | List of foreman settings defined in name:value pairs.                                                                                                  |

- High level structure:
```yaml
satellite:
  foreman:
  katello:
```

- organizations:
```yaml
satellite:
  foreman:
    organizations:
      - name: org_1
        initial_organization: true
      - name: org_2
      - name: org_3
```
- locations:
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
- domains:
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
- subnets:
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
- compute_resources:
```yaml
satellite:
  foreman:
    compute_resources:
      - name: example_vcenter
        provider: vmware
        provider_params:
          url: vcenter.domain1.interna.lcl
          user: provisioning@vsphere.local
          password: "{{ lookup('file', '/tmp/vcenter-password') }}"
          datacenter: dc1
        organizations:
          - name: org_1
          - name: org_2
        locations:
          - name: loc_1
          - name: loc_3
```
- compute_profiles:
```yaml
satellite:
  foreman:
    compute_profiles:
      - name: general-vm
        compute_resource: example_vcenter
        vm_attrs:
          cpus: 2
          corespersocket: 2
          memory_mb: 2048
          firmware: bios
          cluster: general
          resource_pool: Resources
          path: /Datacenters/dc1/vm
          guest_id: rhel7_64Guest
          hardware_version: vmx-13
          memoryHotAddEnabled: 1
          cpuHotAddEnabled: 1
          add_cdrom: 0
          scsi_controllers:
           - type: ParaVirtualSCSIController
             key: 1000
          interface_attributes:
            0:
              type: VirtualVmxnet3
              network: virtaul-machines
          volumes_attributes:
            0:
              thin: true
              name: Hard disk
              mode: persistent
              controller_key: 1000
              size_gb: 100
              datastore: datastore1
```
        
