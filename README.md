## CloudFlare attributes to be Set

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
