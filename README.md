freebsd-pf
==========

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-pf.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-pf)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd-pf/)  Configure FreeBSD PF firewall.

As it manipulates the firewall, there is a risk of being locked out. It's necessary to read the handbook.
- https://www.freebsd.org/doc/en/books/handbook/firewalls-pf.html
- https://www.openbsd.org/faq/pf/filter.html


Requirements
------------

No requiremenst.


Variables
---------

By default the filter is dissabled.

```
pf_enable: False
```

By default [sshguard](https://www.sshguard.net/) and [blacklistd](https://www.freebsd.org/cgi/man.cgi?query=blacklistd) are dissabled.

```
sshguard_enable: False
pf_blacklistd_enable: False
```

TBD. Check the defaults and examples in vars.


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

- [How to Install Blacklistd on FreeBSD 11.1](https://www.vultr.com/docs/how-to-install-blacklistd-on-freebsd-11-1)


License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)

Author Information
------------------

[Vladimir Botka](https://botka.link)
