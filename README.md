# freebsd_pf

[![quality](https://img.shields.io/ansible/quality/27910)](https://galaxy.ansible.com/vbotka/freebsd_pf)[![Build Status](https://app.travis-ci.com/vbotka/ansible-freebsd-pf.svg?branch=master)](https://app.travis-ci.com/vbotka/ansible-freebsd-pf)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_pf/) FreeBSD. Configure PF firewall.

Feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-pf/issues).

[Contributions are welcome](https://github.com/firstcontributions/first-contributions).


Warning:

As it manipulates the firewall, there is a risk of being locked out. It's necessary to read the handbook
- https://www.freebsd.org/doc/en/books/handbook/firewalls-pf.html
- https://www.openbsd.org/faq/pf/filter.html

When you experiment with pf on a remote host open a session and, as root, run the below command before you enable pf

```bash
shell> sleep 120; pfctl -d
```

This will disable pf after 2 minutes and let you to open new session to this remote host in case something went wrong.


## Requirements and dependencies

### Collections

* community.general

### Optional dependencies

- [security/sshguard](https://www.freshports.org/security/sshguard/). See [sshguard fails to start](https://mail-index.netbsd.org/netbsd-users/2018/05/21/msg020789.html)
- [security/py-fail2ban](https://www.freshports.org/security/py-fail2ban/)
- [net/relayd](https://www.freshports.org/net/relayd/)


## Role Variables

By default the firewall is disabled

```yaml
pf_enable: False
```

By default [sshguard](https://www.sshguard.net/),
[blacklistd](https://www.freebsd.org/cgi/man.cgi?query=blacklistd),
[fail2ban](https://www.fail2ban.org/), and
[relayd](https://www.freshports.org/net/relayd/) are disabled

```yaml
pf_blacklistd_enable: False
pf_fail2ban_enable: False
pf_relayd_enable: False
pf_sshguard_enable: False
```

By default blocked packages are not logged

```yaml
pf_log_all_blocked: False
```

Review the defaults and examples in vars.


## Workflow

1) Change shell to /bin/sh

```bash
shell> ansible srv.example.com -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```


2) Install the role and collections

```bash
shell> ansible-galaxy role install vbotka.freebsd_pf
shell> ansible-galaxy collection install community.general
```

3) Create firewall

There are couple of templates *-pf.conf.j2 available. Pick a template
that fits your purpose (router, server, balancer, ...) and review the
examples of the variables *.yml.sample in vars. The template
default-pf.conf.j2 and the variables
pfconf_example*_default.yml.sample are tested. Other templates and
samples will need more attention and testing.

Note: You can create nested lists (tables, options, normalization,
queuing, translation, filtering, ...) for the template
default-pf.conf.j2. The lists will be flattened.


4) Create playbook

```yaml
shell> cat freebsd-pf.yml
- hosts: srv.example.com
  roles:
    - vbotka.freebsd_pf
```

5) Review handlers to see how start/restart/reload of pf is implemented

6) Install packages

```bash
shell> ansible-playbook -t pf_packages  -e pf_install=true freebsd-pf.yml
```

Then, disable the installation to speed up the execution of the playbook.

7) Configure the firewall

Starting and restarting of the firewall breaks the ssh connections. See
the handlers for details. As a consequence, both handlers starting and
reloading don't work properly and the ssh connection will
stale. Therefore, let us first configure the rules

```bash
shell> ansible-playbook -e pf_enable=false freebsd-pf.yml
```

Before you enable the firewall open a ssh session to the remote host
and run the below commands

```bash
shell> sleep 120; pfctl -d
```

If you lock yourself out of the remote host the above commands will
disable the firewall in 2 minutes. You might want to run these
commands always when you experiment with the firewall. Now enable the
firewall

```bash
shell> ansible-playbook -e pf_enable=true freebsd-pf.yml
```


## Update the firewall

Open ssh connection to the host for the case that something goes
wrong. Update and validate the configuration. Do not reload the rules

```bash
shell> ansible-playbook -e pf_conf_only=true -e pf_conf_validate=true freebsd-pf.yml
```

Reload the rules after the configuration has been updated and validated

```bash
shell> ansible srv.example.com -m service -a "name=pf state=reloaded"
```


## Troubleshooting

### pf.conf

As a first step enable backup of the configuration files

```yaml
pf_backup_conf: true
```

In case the configuration /etc/pf.conf does not pass the validation the play fails

```yaml
TASK [vbotka.freebsd_pf : pfconf: Configure rules] **********************************************
fatal: [srv.example.com]: FAILED! => changed=false
  checksum: 765302b1f0de9f200b2cab396e0271fc04e6adcc
  exit_status: 1
  msg: failed to validate
  stderr: |-
    /home/freebsd/.ansible/tmp/ansible-tmp-1554558267.39-44232067735996/source:119: syntax error
```

The message above shows the location of the syntax error (source:119)
in the temporary file created by the template module. It's difficult
to find the error if this temporary file is not available for a
review.

Enable *pf_conf_only=true* and disable validation *pf_conf_validate=false* to find the problem

```bash
shell> ansible-playbook -e pf_conf_only=true -e pf_conf_validate=false freebsd-pf.yml
```

Locate the syntax error in the configuration file /etc/pf.conf

```bash
shell> pfctl -n -f /etc/pf.conf
```

Update, validate, and reload the rules after the configuration was fixed

```bash
shell> ansible srv.example.com -m service -a "name=pf state=reloaded"
```

### relayd.conf

The same way it is possible to troubleshoot /usr/local/etc/relayd.conf

```bash
shell> ansible-playbook -t pf_relayd -e pf_debug=true -e pf_relayd_conf_validate=false -e pf_relayd_conf_only=true freebsd-pf.yml
```
Locate the syntax error in the configuration file /usr/local/etc/relayd.conf

```bash
shell> relayd -n -f /usr/local/etc/relayd.conf
```

Update, validate, and reload the rules after the configuration was fixed

```bash
shell> ansible srv.example.com -m service -a "name=relayd state=reloaded"
```


## Security

To prevent not-validated configuration to be reloaded by the handler
configuration file /etc/pf.conf won't be created and the play will be
terminated if both *pf_conf_only=false* and *pf_conf_validate=false*

```shell
shell> ansible-playbook -e pf_conf_only=false -e pf_conf_validate=false freebsd-pf.yml
fatal: [srv.example.com]: FAILED! => changed=false
  msg: Validation can be turned off if pf_conf_only=True. End of play.
```

The role fails the same way in case `-e pf_relayd_conf_only=false -e pf_relayd_conf_validate=false`


## References

- [Book of PF](https://nostarch.com/pf3)
- [Firewalling with OpenBSD's PF packet filter](http://rlworkman.net/howtos/OpenBSD_pf_guide.html)
- [A Beginner's Guide To Firewalling with pf](http://srobb.net/pf.html)
- [PF Testing Your Firewall - OpenBSD Journal](http://undeadly.org/cgi?action=article&sid=20060928081238)
- [Quick setup of jail on ZFS using ezjail with PF NAT](https://forums.freebsd.org/threads/howto-quick-setup-of-jail-on-zfs-using-ezjail-with-pf-nat.30063/)
- [FreeBSD and blacklistd](https://www.cryptomonkeys.com/2018/05/freebsd-blacklistd/)
- [Blacklistd on FreeBSD](https://www.vultr.com/docs/how-to-install-blacklistd-on-freebsd-11-1)
- [PF and sshguard](https://forums.freebsd.org/threads/how-to-get-pf-and-sshguard-to-stop-this-guy.64933/)
- [Enforcing Fail2ban bans with PF](https://dbdemon.com/pf_and_fail2ban/)
- [Fail2Ban with PF](http://www.purplehat.org/?page_id=566)
- [Fail2Ban pf.conf: fix multiport syntax](https://github.com/fail2ban/fail2ban/pull/1925)


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.info)
