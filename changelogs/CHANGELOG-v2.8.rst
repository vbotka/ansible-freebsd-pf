===================================
vbotka.freebsd_pf 2.8 Release Notes
===================================

.. contents:: Topics


2.8.4
=====

Release Summary
---------------
Maintenance udpate.

Major Changes
-------------

Minor Changes
-------------
* Fix lint complains about file premissions in contrib/freebsd-pf-setup.yml
* Fix CHANGELOG.md


2.8.3
=====

Release Summary
---------------
Feature udpate.

Major Changes
-------------

Minor Changes
-------------
* Enable include files in pf.conf
* Add variables pf_includes_dir and pf_includes
* Add template default3-pf.conf.j2
* Add tasks pfconf-includes.yml
* Add contrib playbook freebsd-pf-setup.yml
* Add sample configuration vars/pfconf_default_ex04.yml.sample


2.8.2
=====

Release Summary
---------------
Maintenance udpate.

Major Changes
-------------

Minor Changes
-------------
* Update README


2.8.1
=====

Release Summary
---------------
Ansible 2.20 upgrade

Major Changes
-------------

Minor Changes
-------------
* Supported versions: 13.5, 14.3, 15.0
* Put ansible facts into the dictionary ansible_facts


2.8.0
=====

Release Summary
---------------
Ansible 2.19 upgrade.

Major Changes
-------------
* Meta: Ansible 2.19; FreeBSD 13.5, 14.2, and 14.3

Minor Changes
-------------
* Explicit bool conversion is not needed.
* Explicit conversion of GroupTouple to lists.

Breaking Changes / Porting Guide
--------------------------------
