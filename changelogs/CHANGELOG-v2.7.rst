===================================
vbotka.freebsd_pf 2.7 Release Notes
===================================

.. contents:: Topics


2.7.6
=====

Release Summary
---------------
Update sshguard configuration.

Major Changes
-------------

Minor Changes
-------------
* Add configuration of /usr/local/etc/sshguard.conf
* Update configuration of /usr/local/etc/sshguard.whitelist
* Add variables: pf_sshguard_conf, pf_sshguard_whitelist_items,
  pf_sshguard_whitelist_content
* Update defaults/main/sshguard.yml
* Update templates/server*-pf.conf.j2
* Update vars/pf_examples.yml.sample
* Update tasks/debug.yml and README.md

Breaking Changes / Porting Guide
--------------------------------
* Variable pf_ssh_whitelist renamed to pf_sshguard_whitelist


2.7.5
=====

Release Summary
---------------
Update README.


2.7.4
=====

Release Summary
---------------
Maintenance update.

Major Changes
-------------

Minor Changes
-------------
* New template default2-pf.conf.j2
* New variable pf_blocks lists block used in the template default2-pf.conf.j2
* Variable pf_packages_install used for pkgng and portinstall.
* Update handlers formatting.
* Add all options to module pkgng in tasks/packages.yml
* Update tasks fail2ban.yml, relayd.yml, and sshguard.yml. Put service
  test into separate files.

2.7.3
=====

Release Summary
---------------

Major Changes
-------------

Minor Changes
-------------
* Tasks blacklistd.yml, fail2ban.yml, relayd.yml, sshguard.yml
  Test service exists in a block.
  Create configuration without pf_*_enable condition
  Update handlers notifications
* Tasks rcconf-blacklistd.yml, rcconf-fail2ban.yml, rcconf-relayd.yml, rcconf-sshguard.yml
  Get and test pf_*_stat if undefined
  Update handlers notifications
* Handlers blacklistd.yml, fail2ban.yml, relayd.yml, sshguard.yml
  Renamed from 'Enable and start \*' to 'Start \*'
  Removed redundant option 'enabled: true/false'.
  Removed redundant conditions.

Breaking Changes / Porting Guide
--------------------------------
Tasks blacklistd.yml, fail2ban.yml, relayd.yml, sshguard.yml

* Create configuration without pf_*_enable condition.
* Since 2.7.1 these tasks are by default disabled completely. See variables
  pf_blacklistd, pf_fail2ban, pf_relayd, pf_sshguard (default=false)


2.7.2
=====

Release Summary
---------------

Major Changes
-------------

Minor Changes
-------------
* Update README


2.7.1
=====

Release Summary
---------------
Add DROP (Don't Route Or Peer Lists) pf_drop_lasso (default=false).
BACKWARD NOT COMPATIBLE: By default, all services are disabled:
pf_blacklistd, pf_fail2ban, pf_relayd, pf_sshguard (default=false).

Major Changes
-------------
* Add vars: pf_blacklistd, pf_fail2ban, pf_relayd, pf_sshguard (default=false)

Minor Changes
-------------
* Add tasks/drop-lasso.yml
* Add defaults/main/drop_lasso.yml
* Add templates/default-drop-lasso.j2
  Script to update Spamhaus Lasso Spam Database for PF Firewall.
* Add option state to module sysrc
* Update tasks/debug.yml
* Update README
* changelog moved to changelogs

Breaking Changes / Porting Guide
--------------------------------
Add vars: pf_blacklistd, pf_fail2ban, pf_relayd, pf_sshguard (default=false)

* By default, the related tasks will be skipped.
* By enabling these "main switches" fit the defaults to your needs. For example,
  review the lists pf_*_rcconf
* These defaults are not backward compatible. Enable these "main switches" if you want to configure
  the services. For example, if you already enabled ``pf_blacklistd_enable=true`` add
  ``pf_blacklistd=true``.


2.7.0
=====

Release Summary
---------------
Maintenance update. Enable /etc/rc.conf.d/

Major Changes
-------------
* Meta: Ansible version 2.18
* Meta: FreeBSD versions 13.4, 14.1, and 14.2

Minor Changes
-------------
* Enable /etc/rc.conf.d/ in all tasks/rcconf-\*.yml

Breaking Changes / Porting Guide
--------------------------------
* The variable pf_use_rc_conf_d renamed to pf_rcconfd
* Lists \*_rcconf changed attributes names.
  * The attribute regexp renamed to name
  * The attribute line renamed to value
