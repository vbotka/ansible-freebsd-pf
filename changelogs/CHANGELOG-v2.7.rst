===================================
vbotka.freebsd_pf 2.7 Release Notes
===================================

.. contents:: Topics


2.7.1
=====

Release Summary
---------------
Add DROP (Don't Route Or Peer Lists).
By default, all disabled (pf_drop_lasso=false).

Major Changes
-------------

Minor Changes
-------------
* Add tasks/drop-lasso.yml
* Add defaults/main/drop_lasso.yml
* Add templates/default-drop-lasso.j2
  Script to update Spamhaus Lasso Spam Database for PF Firewall.
* Update tasks/debug.yml
* Update README references.
* changelog moved to changelogs.


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
