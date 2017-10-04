# Ansible Config Driven Helper
This is an Ansible role that provide configurations tasks and default values for
other Ansible roles which only provide Installation of a particular service or application.

Currently providing support for:
- Hostname configuration
- Environment Groups (system groups)
- SUDOERS configuration files
- CloudFlare configuration

## CloudFlare attributes to be set

DEFINING cloudflare_zone IS _NECESSARY_ & _ENOUGH_ TO CREATE A DNS RECORD
(all other parameters have a default that you can override)
makes exception the "enc_" attributes that need to be defined on a vault file
```
---
# define this in a 'group_var' or 'host_var' depending of the scope of the vm (pipeline)
  cloudflare_zone: whateverzone.some

# define this in a group_var belonging to the organisation
# define enc_* in a vault
  cloudflare_api_email: "{{ enc_cloudflare_api_email }}"
  cloudflare_api_token: "{{ enc_cloudflare_api_email  }}"

# define this in the host_vars
  cloudflare_hostname: "{{ hostname }}"
  cloudflare_record_content: "{{ ansible_ssh_host }}" # IP address or FQDN
  cloudflare_record_type: "A" # or any other DNS record type
...
```
## GROUPS
Most of the user groups needed are generally managed via JumpCloud
In case you need to create groups not managed via JC (or simply don't want to use JC)
you need to define a `user_groups_environment` where you have to list elements of
an attributes array as `inviqa_support_groups` in the example below.
```
inviqa_support_groups:
  support:
    gid: "{{ inviqa_group_ids.support }}"
    name: inviqasupport
  support_ooh:
    gid: "{{ inviqa_group_ids.support_ooh }}"
    name: inviqaoohsupport

user_groups_environment:
- "{{ inviqa_support_groups.support }}"
- "{{ inviqa_support_groups.support_ooh }}"

```
## SUDOERS attributes to be set
Define `sudoers_group_gids` per group or per host. They need to be GID values (not names).
Default attributes values that can be referenced are defined in `inviqa_group_ids` and `support_`, `pipeline_` or `production_`.
The default is set to use PIPELINE values.
```
  sudoers_group_gids:
    inviqa_support:     "{{ inviqa_group_ids.support }}"
    inviqa_support_ooh: "{{ inviqa_group_ids.support_ooh }}"
    admin:              "{{ inviqa_group_ids.pipeline_admin }}"
    deploy:             "{{ inviqa_group_ids.pipeline_deploy }}"
    provision:          "{{ inviqa_group_ids.pipeline_provision }}"
```

## License
[MIT][license]

## Author Information
------------------
Author Marco Massari Calderone - Inviqa UK Ltd

Based on Barney Hanlon's work on the original [ansible-provisioning][ansible-provisioning] project for [Inviqa][inviqa]

[github] https://github.com/inviqa/ansible-config-driven-helper "Github location of this role"
[ansible-provisioning]]: https://github.com/inviqa/ansible-provisioning "Inviqa's Ansible Provisioning tool"

[inviqa]: https://www.inviqa.com "Inviqa UK Ltd"

[license]: https://raw.githubusercontent.com/inviqa/ansible-jumpcloud/master/LICENSE
