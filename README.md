# Ansible-Monit-Slack

Ansible role to install and configure only daemonized services and its dependencies to monitor them with Monit and Slack as an alert service system. Using Service Configuration Templates (SCT) and the possibility to auto generate Basic Service Configuration Templates when the SCT of the specified service are not available.
  
*  [Requirements](#requirements)
*  [Preconfigure Services](#preconfigure-services)
*  [Preconfigure Slack](#preconfigure-slack)
*  [Role Variables](#role-variables)
*  [Install](#install)
*  [Custom Facts](#custom-facts)


## Requirements

-- Debian based system
-- An Ansible ready host
-- Daemonized services

## Preconfigure Services

There are two modes to configure the required services for your system:

**1) Excluding mode:** this means that the user only need to provide the blacklisted services (services that the user **don't** want to monitor) the rest of the services found in the system will be monitored.

**2) Selective mode:** Aside from the fact that the user must provide the blacklist, he/she will also need to provide the whitelist (services that the user **want** to monitor) because the services to be monitored will only be taken given this last list, the rest of services (blacklist and other found services in the system) will not be monitored.

## Preconfigure Slack

Any of the modes you chose you need to get the url of your Slack team channel to post the system alerts, to obtain it you need to create a new Incoming WebHook. You can do that by going to:

* https://\<**yourteam**\>.slack.com/services/new/incoming-webhook
* Chose or create a channel
* Then click on Add incoming WebHooks Integration
* Then you will see a Webhook URL that should be similar to this: https://hooks.slack.com/services/XXXXXXXXX/YYYYYYYYY/zzzzzzzzz

## Role Variables

You need to update the content of this role variables with the value according to your needs. These variables are in the main.yml file of the folder named defaults within the role

*  `monit_config_mode`: Monit Excluding or Selective mode to configure the services.
`monit_config_mode: yes`= Excluding mode  
`monit_config_mode: no`= Selective mode
*  `monit_blacklist`: A list of service names who the user that **don't** want to monitor
*  `monit_whitelist`: A list of service names who the user that **want** to monitor
*  `slack_url`: a WebHook url of your Slack team channel

## Install

Run it in a playbook with a global `become: yes` like:
```yaml
- { role: Ansible-Monit-Slack, become: yes, tags: Monit }
```
Or invoke the role in your playbook like:
```yaml
- hosts: foo
  roles:
	- role: Ansible-Monit-Slack
	  become: yes
```
## Custom Facts

This role writes a log file about the results of a successful installation, showing a summary of the services configured and what mode was used to do it. The log file can be found in the path `/etc/monit/rs.log`
