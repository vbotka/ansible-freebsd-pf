===================================
vbotka.freebsd_pf 2.7 Release Notes
===================================

.. contents:: Topics


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
