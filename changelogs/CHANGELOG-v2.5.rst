===================================
vbotka.freebsd_pf 2.5 Release Notes
===================================

.. contents:: Topics


2.5.0
=====


Release Summary
---------------
Ansible 2.15 update. Added support for relayd. Unified namespace
pf_*. Added universal template default-pf.conf.j2.


Major Changes
-------------
- Add support for net/relayd
- Add templates: default-pf.conf.j2, default-relayd.conf.j2, default-ini.j2
- Add dictionaries: pf_relayd_macros, pf_relayd_redirections, pf_relayd_protocols, pf_relayd_relays
- Add lists: pf_relayd_tables, pf_relayd_options
- Add variables: pf_relayd_conf_only, pf_relayd_conf_validate and pf_relayd_conf_validate_command
- Add dictionary pf_macros
- Add lists: pf_tables, pf_options, pf_normalization, pf_queuing,
  pf_translation, pf_anchors, and pf_filtering
- Merge all installed packages into a single list. Add var pf_packages_install.
- Updated README


Minor Changes
-------------
- Add variables: pf_blacklistd_template, pf_conf_template
- Add examples vars/pfconf_example*.yml.sample
- Backup blacklistd.conf.orig
- Unified error tag [ERR]
- Update output of debug
- Update pfconf block and task names


Breaking Changes / Porting Guide
--------------------------------
- Templates server*-pf.conf.j2 and router*-pf.conf.j2 do not work with
  defaults/pfconf.yml anymore. See templates/default-pf.conf.j2 on how
  to update templates.
- Rename variables pflog_* to pf_log_*
- Rename variables pfconf_only, pfconf_validate,
  pfconf_validate_command to pf_conf_only, pf_conf_validate,
  pf_conf_validate_command
- Rename templates/blacklistd.conf.j2 to
  templates/default-blacklistd.conf.j2
- Rename vars/main.yml.sample to vars/pf_examples.yml.sample
- Updated defaults
- Configuration of fail2ban not ready for v1.0


Bufixes
-------
- fail2ban - fix defaults and templates
