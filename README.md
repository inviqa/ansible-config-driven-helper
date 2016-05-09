## Digital Ocean attributes that can be set
All the attributes have a default value set
to override the default values add the following attributes as a group_var or host_var
```
  digital_ocean_api_token:          
  digital_ocean_size_id:            
  digital_ocean_image_id:           
  digital_ocean_name:               
  digital_ocean_region_id:          
  digital_ocean_unique_name:        
  digital_ocean_backups_enabled:    
  digital_ocean_private_networking:
  digital_ocean_ssh_key_ids:        
```
## CloudFlare attributes to be set

DEFINING cloudflare_zone IS ENOUGH TO CREATE A DNS RECORD
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
in case you need to create groups not managed via JC or simply don't want to use JC
you can create groups defining the `inviqa_support_groups` and `user_groups_environment` attributes
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
## SUDOERS attributes to be se
Define `sudoers_group_gids` per group or per host.
It's possible to choose `inviqa_group_ids` or `inviqa_alternate_group_ids` and `pipeline_x` or `prouction_x`.
The default is set to use PIPELINE values.
```
  sudoers_group_gids:
    inviqa_support:     "{{ inviqa_group_ids.support }}"
    inviqa_support_ooh: "{{ inviqa_group_ids.support_ooh }}"
    admin:              "{{ inviqa_group_ids.pipeline_admin }}"
    deploy:             "{{ inviqa_group_ids.pipeline_deploy }}"
    provision:          "{{ inviqa_group_ids.pipeline_provision }}"
```
