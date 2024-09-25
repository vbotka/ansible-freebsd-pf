===================================
vbotka.freebsd_pf 2.6 Release Notes
===================================

.. contents:: Topics
# BEGIN Commits 2.6.2
- Update python 3.11 in .travis.yml
- Rename changelogs to changelog.
- Update tests/test.yml playbook
- Start devel 2.6.2
# END Commits 2.6.2
# BEGIN Release notes 2.6.2
2.6.2
=====
Release Summary
---------------
Major Changes
-------------
Minor Changes
-------------
- Update python 3.11 in .travis.yml
- Rename changelogs to changelog.
- Update tests/test.yml playbook
- Start devel 2.6.2

Bugfixes
--------
Breaking Changes / Porting Guide
--------------------------------
# END Release notes 2.6.2


2.6.2
=====

Release Summary
---------------
Maintenance update.

Major Changes
-------------

Minor Changes
-------------
* Update tests/test.yml playbook


2.6.1
=====

Release Summary
---------------
Ansible 2.17 update.

Major Changes
-------------
* Supported 13.3, 14.0, and 14.1

Minor Changes
-------------
* Update README.
* Update conf and fix lint.
* Update debug.
* Update handlers. Listen to lowercase names.
* Add var pf_role_version


2.6.0
=====

Release Summary
---------------
Ansible 2.16 update.

Major Changes
-------------
* Supported versions: 12.4, 13.2, 14.0
* Add dictionary pf_packages. Update declaration of
  pf_packages_install

Minor Changes
-------------

Bufixes
-------
* Fix template default-ini.j2

Breaking Changes / Porting Guide
--------------------------------
