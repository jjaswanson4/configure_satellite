configure_certificates
=========

Configure CA-signed certificates on a Satellite server or Capsule.

Requirements
------------

Inventory Groups:
- satellite
- capsules

Variables:

```
---
certificates:
  hostname1:
    key_content: "TLS certificate KEY file contents"
    crt_content: "TLS certificate CRT file contents"
    bundle_path: "full path to CA bundle on satellite and capsule server"
  hostname2:
    key_content: ""
    crt_content: ""
    bundle_path: ""
  ...
  ...

```

Example Variables
===

Certificates stored in Hashicorp Vault:

```
---

certificates:
  satellite.example.com:
    key_content: "{{ lookup('hashi_vault', 'secret=secret/data/satellite.example.com url=https://hashicorpvault.example.com validate_certs=True cacert=/etc/ssl/certs/ca-bundle.crt')['key']}}"
    crt_content: "{{ lookup('hashi_vault', 'secret=secret/data/satellite.example.com url=https://hashicorpvault.example.com validate_certs=True cacert=/etc/ssl/certs/ca-bundle.crt')['crt']}}"
    bundle_path: /etc/pki/ca-trust/source/anchors/ca.pem
  capsule.example.com:
    key_content: "{{ lookup('hashi_vault', 'secret=secret/data/capsule.example.com url=https://hashicorpvault.example.com validate_certs=True cacert=/etc/ssl/certs/ca-bundle.crt')['key']}}"
    crt_content: "{{ lookup('hashi_vault', 'secret=secret/data/capsule.example.com url=https://hashicorpvault.example.com validate_certs=True cacert=/etc/ssl/certs/ca-bundle.crt')['crt']}}"
    bundle_path: /etc/pki/ca-trust/source/anchors/ca.pem
```

Encrypted Ansible Vault:

```
```
