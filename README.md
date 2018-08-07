# Ansible-Monit-Slack

**WARNING.** This role although functional is still under construction and improvements therefore it may fail in some cases

Ansible role to install and configure daemonized-only services and his dependencies to monitor them with Monit, and use Slack as an alert system. Using Service Configuration Templates (SCT) and the possibility to auto generate Basic Service Configuration Templates when the SCT of the specified service are not available.

* [Requirements](#requirements)
* [Role Variables](#role-variables)
* [Preconfigure Services](#preconfigure-services)
* [Preconfigure Slack](#preconfigure-slack)
* [Install](#install)
* [Log information](#log-information)
* [License](#license)

## Requirements

   -- Debian: jessie (8.10) and Ubuntu: Xenial (16.04)  
   -- An Ansible ready host (Ansible 2.4 or up)  
   -- systemd  
   -- Daemonized and running services

## Role Variables

Update the role variables accordingly to your needs. These variables are in the main.yml file of the defaults folder within the role

* `monit_config_mode`: Monit Excluding or Selective mode to configure the services (more info about this in the "Preconfigure services" section).  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Excluding mode = `monit_config_mode: yes`  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Selective mode = `monit_config_mode: no`
* `monit_blacklist`: A list of service names who the user **don't** want to monitor
* `monit_whitelist`: A list of service names who the user **want** to monitor
* `slack_url`: a WebHook url of your Slack team channel

## Preconfigure Services

There are two modes for configure the required services for your system:

**1) Excluding mode:** this means that the user only need to provide the blacklisted services (services who the user **don't** want to monitor) the rest of the services found in the system will be monitored.

**2) Selective mode:** Aside from the fact that the user must provide the blacklist, he will also need to provide the whitelist (services who the user **want** to monitor) because the services to be monitored will only be taken given this last list, the rest of services (blacklist and other found services in the system) will not be monitored.

Update the `monitrc` template file located in `templates` folder according to your needs.

Update the content of the each service template file that you want to monitor, located in `templates/conf-available` folder according to your needs. If no template is found related to your service, you can add the new template taking as an example the structure of the existing templates, no problem if you do not add, the system will automatically create a basic configuration template for your service.

## Preconfigure Slack

For any mode, an url of the Slack team channel is needed to post the system alerts; in order to obtain it, the creation of a new Incoming WebHook is required. To do so, go to:

* `https://<yourteam>.slack.com/services/new/incoming-webhook`
* Choose or create a channel
* Then click on Add incoming WebHooks Integration
* Then you will see a Webhook URL that should be similar to this: `https://hooks.slack.com/services/XX/YY/zz`

## Install

Run it in a playbook with a global `become: yes` like:

```yaml
- { role: ansible-monit-slack, become: yes, tags: monit }
```

Or invoke the role in your playbook like:

```yaml
- hosts: foo
  roles:
   - role: ansible-monit-slack
     become: yes
```

## Log information

If the installation process went without any problems you can find the log file in the path `/etc/monit/rs.log` with a summary about all services configured in the process.

## license

Licensed under the [GPL-3.0](https://github.com/cracos/ansible-monit-slack/blob/master/LICENSE) license
