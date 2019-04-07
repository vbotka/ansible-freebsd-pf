freebsd_pf
==========

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-pf.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-pf)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_pf/) FreeBSD. Configure PF firewall.

As it manipulates the firewall, there is a risk of being locked out. It's necessary to read the handbook.
- https://www.freebsd.org/doc/en/books/handbook/firewalls-pf.html
- https://www.openbsd.org/faq/pf/filter.html


Requirements
------------

No requirements.


Variables
---------

By default the firewall is disabled.

```
pf_enable: False
```

By default [sshguard](https://www.sshguard.net/), [blacklistd](https://www.freebsd.org/cgi/man.cgi?query=blacklistd) and [fail2ban](https://www.fail2ban.org/) are disabled.

```
pf_blacklistd_enable: False
pf_fail2ban_enable: False
pf_sshguard_enable: False
```

TBD. Review the defaults and examples in vars.


Workflow
--------

1) Change shell to /bin/sh.

```
# ansible srv.example.com -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
# ansible-galaxy install vbotka.freebsd_pf
```

3) Fit variables.

```
# editor vbotka.freebsd_pf/vars/main.yml
```

4) Create playbook.

```
# cat freebsd-pf.yml
- hosts: srv.example.com
  roles:
    - vbotka.freebsd_pf
```

5) Review handlers to see how start/restart/reload of pf is implemented.

6) Configure the firewall.

Starting and restarting of the firewall breaks the ssh connection. See
the handlers for details. Both handlers starting and reloading in
consequence doesn't work properly and the ssh connection will
stale. Therefore let us first configure the rules

```
# ansible-playbook -e 'pf_enable=False' freebsd-pf.yml
```

and enable the firewall in the second step

```
ansible-playbook -e 'pf_enable=True' freebsd-pf.yml
```

Update the firewall.
--------------------

Open a ssh connection to the host for the case that something goes
wrong. Update and validate the configuration. Do not reload the rules.

```
ansible-playbook -e 'pfconf_only=yes pfconf_validate=yes' freebsd-pf.yml
```

After the configuration has been updated and validated reload the rules.

```
ansible srv.example.com -m service -a "name=pf state=reloaded"
```

Troubleshooting
---------------

As a first step enable backup of the configuration files.

```
pf_backup_conf: yes
```

In case the configuration does not pass the validation the play stops.

```
TASK [vbotka.freebsd_pf : template] **********************************************
fatal: [srv.example.com]: FAILED! => changed=false
  checksum: 765302b1f0de9f200b2cab396e0271fc04e6adcc
  exit_status: 1
  msg: failed to validate
  stderr: |-
    /home/freebsd/.ansible/tmp/ansible-tmp-1554558267.39-44232067735996/source:119: syntax error
```

The message above shows the location of the syntax error (source:119)
in the temporary file created by the template module. It's difficult to
find the error if this temporary file is not available for a review.

An option how to easier find the problem is to enable
*pfconf_only=yes* and disable validation *pfconf_validate=no*

```
ansible-playbook -e 'pfconf_only=yes pfconf_validate=no' freebsd-pf.yml
```

Created configuration file /etc/pf.conf contains the syntax error
which can be located and found in the file /etc/pf.conf.

```
pfctl -n -f /etc/pf.conf
```

After the configuration has been fixed, updated and validated reload the rules.

```
ansible srv.example.com -m service -a "name=pf state=reloaded"
```

Security
--------

To prevent not validated configuration will be reloaded by the
handler, configuration file /etc/pf.conf won't be created and the play
will be ended if both *pfconf_only=no* and *pfconf_validate=no*

```
ansible-playbook -e 'pfconf_only=no pfconf_validate=no' freebsd-pf.yml
fatal: [srv.example.com]: FAILED! => changed=false
  msg: Validation can be turned off if pfconf_only=yes. End of play.
```

References
----------

- [Firewalling with OpenBSD's PF packet filter](http://rlworkman.net/howtos/OpenBSD_pf_guide.html)
- [A Beginner's Guide To Firewalling with pf](http://srobb.net/pf.html)
- [Blacklistd on FreeBSD](https://www.vultr.com/docs/how-to-install-blacklistd-on-freebsd-11-1)
- [PF and sshguard](https://forums.freebsd.org/threads/how-to-get-pf-and-sshguard-to-stop-this-guy.64933/)
- [Fail2Ban with PF](http://www.purplehat.org/?page_id=566)
- [Fail2Ban pf.conf: fix multiport syntax](https://github.com/fail2ban/fail2ban/pull/1925)

License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)

Author Information
------------------

[Vladimir Botka](https://botka.link)
