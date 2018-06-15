freebsd-pf
==========

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-pf.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-pf)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd-pf/)  Configures FreeBSD PF firewall.

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

By default [sshguard](https://www.sshguard.net/) is dissabled.
```
sshguard_enable: False
```

TBD (Check the defaults).


Workflow
--------

1) Change shell to /bin/sh.

```
ansible do-bsd-test -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
ansible-galaxy install vbotka.freebsd-pf
```

3) Fit variables.

```
~/.ansible/roles/vbotka.ansible-freebsd-pf/vars/main.yml
```

4) Create playbook.

```
> cat ~/.ansible/playbooks/freebsd-pf.yml
---
- hosts: do-bsd-tetst
  become: yes
  become_method: sudo
  roles:
    - role: vbotka.freebsd-pf
```

5) Review handlers to see how start/restart/reload of pf is implemented.

6) Configure the firewall.

Starting and restarting of the firewall breaks the ssh connection. See the handlers for details. Both handlers starting and reloading in consequence doesn't work properly and the ssh connection will stale. Therefor let us first configure the rules

```
ansible-playbook -e 'pf_enable=False' ~/.ansible/playbooks/freebsd-pf.yml
```

and enable the firewall in the second step

```
ansible-playbook -e 'pf_enable=True' ~/.ansible/playbooks/freebsd-pf.yml
```

License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)

Author Information
------------------

[Vladimir Botka](https://botka.link)
