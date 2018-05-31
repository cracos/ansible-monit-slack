# Ansible-Monit-Slack

Ansible role for install and configure only services and his dependencies to monitor with Monit, and Slack as an alert service system. Using Service Configuration Templates (SCT) and the possibility to auto generate Basic Service Configuration Templates when the SCT of the specified service are not available.

* [Requirements](#requirements)
* [Preconfigure Services](#preconfigure-services)
* [Preconfigure Slack](#preconfigure-slack)
* [Role Variables](#role-variables)
* [Install](#install)


## Requirements

    -- Debian based system
    -- An Ansible ready host
    -- Daemonized services

## Preconfigure Services

There are two ways for configure the required services for your system:

**1) Excluding way:** this means that the user only need to provide the blacklisted services (services who the user **don't** want to monitor) the rest of the services found in the system will be monitored.  
**2) Selective way:** Aside from the fact that the user must provide the blacklist, he will also need to provide the whitelist (services who the user **want** to monitor) because the services to be monitored will only be taken given this last list, the rest of services (blacklist and other found services in the system) will not be monitored.

## Preconfigure Slack

Any of the ways you chose you need to get the url of your Slack team channel for post the system alerts,  to obtain it you need to create a new Incoming WebHook. You can do that by going to: 
* https://\<yourteam\>.slack.com/services/new/incoming-webhook
* Coose or create a channel
* Then click on Add incoming WebHooks Integration
* Then you will see a Webhook URL that should be similar to this: https://hooks.slack.com/services/XXXXXXXXX/YYYYYYYYY/zzzzZzzzzzzzZzzzzzzzzzzzz


## Role Variables

* `monit_eos`: Monit Excluding or Selective way to configure the services. `monit_eos: yes`= Excluding way. `monit_eos: no`= Selective way
* `monit_blacklist`: 
* `monit_whitelist`:

## Install

Run it in a playbook with a global `become: yes` like: 

    - { role: Ansible-Monit-Slack, become: yes, tags: Monit }

Or invoke the role in your playbook like:
```yaml
- hosts: foo
  roles:
    - role: Ansible-Monit-Slack
      become: yes
```
