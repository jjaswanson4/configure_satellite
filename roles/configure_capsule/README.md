# configure_capsule
=========

Ansible role to configure a Satellite Capsule.

Requirements
------------

Ansible Groups:
- satellite (used for delegation)
- capsules


Global Variables:

```
satellite:
  foreman:
    organizations:
      - name: exampleorg
        ...
        ...
      - name: exampletwo
        ...
        ...
```

Host or Group Variables:

(These can be defined at either the host or group level)

Capsule Location (string)

```
capsule_location: exameplelocation

```

Capsule Environments (list)
```
capsule_environments:
  - Library
  - Dev
  - Example
  - Prod

```

