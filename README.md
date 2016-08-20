freebsd-pf
==================

This role configures FreeBSD PF firewall.

As it manipulates the firewall, there is a risk of being locked
out. It's necessary to read the handbook.
- https://www.freebsd.org/doc/en/books/handbook/firewalls-pf.html
- https://www.openbsd.org/faq/pf/filter.html


Requirements
------------

No requiremenst.


Variables
---------

TBD (Check the defaults).


Workflow
--------

1) Change shell to /bin/sh.

```
ansible do-bsd-test -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
ansible-galaxy install vbotka.ansible-freebsd-pf
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
    - role: vbotka.ansible-freebsd-pf
```

5) Review handlers to see how start/restart/reload of pf is implemented.

6) Configure the firewall.

```
ansible-playbook ~/.ansible/playbooks/freebsd-pf.yml
```


License
-------

BSD


Author Information
------------------

[Vladimir Botka](https://botka.link)
