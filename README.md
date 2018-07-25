freebsd-pf
==========

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-pf.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-pf)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd-pf/) FreeBSD. Configure PF firewall.

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
# ansible-galaxy install vbotka.freebsd-pf
```

3) Fit variables.

```
# editor vbotka.freebsd-pf/vars/main.yml
```

4) Create playbook.

```
# cat freebsd-pf.yml
- hosts: srv.example.com
  roles:
    - vbotka.freebsd-pf
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

References
----------

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
