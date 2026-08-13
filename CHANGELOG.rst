=================================================
foundata.postfix Ansible collection Release Notes
=================================================

.. contents:: Topics

v2.0.1
======

Release Summary
---------------

Release Date: 2026-08-13

Bugfix release.

Bugfixes
--------

- boolean role arguments are now coerced with ``ansible.builtin.bool`` in every conditional expression. String values, as delivered by ``-e var=false`` command line extra-vars, were evaluated by truthiness before, so ``"false"`` enabled the gated behavior instead of disabling it.

v2.0.0
======

Release Summary
---------------

Release Date: 2026-07-30

Maintenance and bugfix release (with one *potentially* breaking change: Ports 465 (smtps) and 587 (submission) are no longer opened by the default; therefore requires a major version bump according to Semantic Versioning).

Minor Changes
-------------

- Add a production-oriented Postfix playbook example for LDAP-backed Dovecot mailboxes, authenticated submission, LMTP delivery, quota checks, and a mandatory-TLS upstream relay.
- The Molecule ``default`` scenario now selects the test backend per platform via a ``type`` key: ``podman`` (container, the default when omitted) or ``libvirt`` (QEMU/KVM virtual machine from a vendor cloud image via a session libvirt daemon, without root privileges). VM platforms allow tests containers cannot cover; commented ``libvirt`` alternates for every platform are included in ``molecule.yml``. ``molecule login`` now works through a per-instance login command for both backends. See ``extensions/molecule/README.md`` for requirements and usage.
- ``run`` role - Configuration changes (``main.cf``, ``master.cf`` and the Debian mailname file) now notify a service reload instead of a restart, so established connections and the queue manager survive ordinary reconfiguration. Package installations and upgrades now notify a restart instead (new binaries only take effect with one). The previously unreachable reload handler skips services that are not running yet: on fresh installs the service phase then starts Postfix with the new configuration. Documented Postfix exception (see ``postconf(5)``): ``inet_interfaces`` and ``inet_protocols`` changes still require a manual stop/start, a plain "postfix reload" is not sufficient either.
- ``run`` role - Documented that the ``recipient_access`` / ``sender_access`` restriction classes are prepended before ``reject_unauth_destination``, so an ``OK`` action in ``run_postfix_access_recipient_map`` / ``run_postfix_access_sender_map`` accepts the mail immediately and bypasses the relay guard. An unscoped ``OK`` (e.g. ``/^abuse@.+/``) therefore relays to that pattern on any domain (open relay). The examples now use ``DUNNO`` for allow-listing and show a domain-scoped ``OK`` for force-accepting RFC 2142 addresses without relaying. No role behaviour changed; the molecule tests now include an anti-open-relay probe.
- ``run`` role - The ``master.cf`` update no longer creates an empty file when ``/etc/postfix/master.cf`` is missing. Doing so produced a Postfix with only the role-managed services and none of the internal ones needed to deliver mail. The task now fails with a message pointing at the Postfix package.
- ``run`` role - ``run_postfix_mastercf_settings`` service definitions are now validated before ``master.cf`` is written. A service missing one of the required fields (``type``, ``private``, ``unpriv``, ``chroot``, ``wakeup``, ``maxproc``, ``command_args``) previously aborted with a raw templating error that did not name the offending service; the role now reports the service and the missing fields.

Breaking Changes / Porting Guide
--------------------------------

- ``run`` role - Ports 465 (``smtps``) and 587 (``submission``) are no longer opened by the default ``master.cf`` settings. Deployments that relied on the previous defaults must now enable these services explicitly through ``run_postfix_mastercf_settings``. As with every ``master.cf`` service, the role manages entries additively (upsert via ``postconf -M``) and by design never removes services it does not declare, so hosts that already have ``smtps``/``submission`` entries from an earlier run keep them. Remove them manually if desired (e.g. ``postconf -MX smtps/inet``).

Security Fixes
--------------

- ``run`` role - The ``smtps`` (port 465) and ``submission`` (port 587) services are no longer part of the default ``master.cf`` configuration. Previously they were enabled on every host with a bare ``smtpd`` and no per-service overrides, even though the role configures no TLS by default: port 465 (implicit TLS, RFC 8314) was served as a STARTTLS listener without ``smtpd_tls_wrappermode=yes`` (unusable by conforming clients and, absent a certificate, plaintext only), and port 587 was an unauthenticated submission port that did not enforce TLS. Both services are now opt-in via ``run_postfix_mastercf_settings`` and should be configured with a working TLS setup (``run_postfix_maincf_settings``) and the appropriate ``-o`` overrides (see the example in ``defaults/main.yml``).
- ``run`` role - The task writing ``/etc/postfix/smtp_sasl_password`` had no ``no_log``, so a ``--diff`` run printed the relay credentials to the Ansible output and any log capturing it. An ``-vvv`` run additionally printed the merged credential map from ``tasks/init.yml``. The template task is now ``no_log``, ``run_postfix_smtp_sasl_password_map`` is marked ``no_log`` in the argument specification, and the debug output shows only the lookup keys (the relay hosts) instead of the credentials.
- ``run`` role - ``run_postfix_state: absent`` removed the packages but left ``/etc/postfix/smtp_sasl_password`` and its compiled database on disk, keeping relay credentials in clear text on a host the role was told to revert. The uninstall now removes the file and every compiled database variant of it.

Bugfixes
--------

- The comment written into neutralized distribution config files contained a stray double quote in the Debian hint (``dpkg -S '<file>'"``), so the suggested command could not be copied and pasted as-is. The quote is removed.
- ``run`` role - Alias names were interpolated unescaped into the regular expression that comments out pre-existing entries in the aliases file, so metacharacters were interpreted: an alias such as ``new.name`` also commented out unrelated entries like ``new-name``, and one such as ``user+tag`` matched ``usertag`` while missing the intended line. The alias is now passed through ``regex_escape``.
- ``run`` role - Corrected the documentation of the nine ``*_manage`` options (``run_postfix_relay_domains_manage``, ``run_postfix_access_manage``, ``run_postfix_aliases_manage``, ``run_postfix_canonical_manage``, ``run_postfix_generic_manage``, ``run_postfix_relocated_manage``, ``run_postfix_transport_manage``, ``run_postfix_virtual_manage`` and ``run_postfix_smtp_sasl_password_manage``), which stated that ``true`` was the default while all of them ship ``false``. Readers had to assume that the role managed the corresponding lookup tables out of the box. Only the documentation was wrong, the behaviour is unchanged.
- ``run`` role - Fixed two misleading comments in generated table files: the header of ``access_sender`` described its contents as recipient patterns, and ``canonical_recipient`` linked to the ``sender_canonical_maps`` documentation although the file is wired to ``recipient_canonical_maps``.
- ``run`` role - Platform-specific task files are now guaranteed to run before the shared default tasks. The former single include loop did not preserve that order with several platforms in one play: Ansible batches the includes across hosts and the insertion order depends on when results arrive (non-deterministic), so default tasks could run before platform-specific ones. The includes are now two sequential tasks, which is a hard ordering barrier.
- ``run`` role - Pre-existing entries in ``/etc/aliases`` that conflict with the managed block are now disabled with an ``#ANSIBLE_DISABLED`` marker instead of a plain ``#``, and ``run_postfix_state: absent`` restores them and removes the managed block again. Previously the uninstall left the file as the role had rewritten it, so aliases the role had commented out stayed disabled. The marker is what makes the restore safe: a plain ``#`` cannot be told apart from an entry the administrator disabled on purpose, so restoring those would silently re-enable them. ``/etc/aliases`` itself is never removed, it is a system file shared with any other local MTA.
- ``run`` role - The "...config value will be overwritten" notices called ``.split(', ')`` directly on the raw ``run_postfix_maincf_settings`` value. When such a value was supplied as a YAML list (which the documentation explicitly allows), the task failed with "'list object' has no attribute 'split'". The membership check now only splits string values and uses list values as-is.
- ``run`` role - The ``master.cf`` update task quoted only the service type when building the ``postconf -M <service>/<type>=...`` argument, because the Jinja ``| quote`` filter binds tighter than ``~`` and applied to ``fields['type']`` alone. The whole ``<service>/<type>`` expression is now quoted.
- ``run`` role - The ``postmap``/``postalias`` path helpers (``helper_postmap_paths.j2`` and ``helper_postalias_paths.j2``) now detect ``cdb`` maps. The role installs ``postfix-cdb`` and offers ``cdb`` as a table type, but the "needs indexing" regex omitted it, so ``cdb:`` lookup tables were never compiled with ``postmap``/``postalias`` and stayed unusable at runtime.
- ``run`` role - The ``postmap``/``postalias`` path helpers stripped an inline prefix (e.g. ``proxy:``) from a lookup-table value with a regex anchored as ``:/[^\s]$``, which only matched a single-character path and therefore left real paths untouched. Prefixed values such as ``proxy:lmdb:/etc/postfix/map`` reached the handler uncleaned, so ``postmap proxy:...`` failed. The path is now matched with ``:/[^\s]+$``.
- ``run`` role - The ``recipient_access`` and ``sender_access`` ``smtpd_restriction_classes`` are now defined with an explicit ``check_recipient_access`` / ``check_sender_access`` restriction instead of a bare ``<type>:<path>`` map. A bare table in a restriction class inherits the recipient context of ``smtpd_relay_restrictions``, so the sender table was looked up by the recipient address and sender access rules (``run_postfix_access_sender_map``) never took effect. Recipient rules were unaffected, which is why the behaviour went unnoticed. The molecule verify stage now probes sender and recipient rejection over SMTP to guard against a regression.
- ``run`` role - The ``run_postfix_aliases_map``, ``run_postfix_access_recipient_map``, ``run_postfix_access_sender_map``, ``run_postfix_canonical_recipient_map`` and ``run_postfix_canonical_sender_map`` lookup maps were merged in the wrong operand order (``user | combine(defaults)``), so the internal defaults won over user-supplied values. In practice this silently discarded user overrides of the default ``abuse`` / ``postmaster`` / ``mailer-daemon`` aliases. These maps now merge as ``defaults | combine(user)`` like every other map in the role, so user values win and the defaults act as a fallback. The access and canonical maps have empty defaults, so their behavior therefore remains effectively unchanged as the bug did not manifest for them.
- ``run`` role - The default for the ``myhostname`` main.cf setting used ``ansible_facts['hostname']``, which Ansible deliberately truncates at the first dot, so on a host such as ``mail.example.com`` Postfix was configured with the short name ``mail``. Postfix announces ``myhostname`` as the EHLO/HELO name, so remote MTAs using ``reject_non_fqdn_helo_hostname`` rejected the mail; the value also seeds ``/etc/mailname`` and the Debian debconf ``mailname``, and Postfix derives ``$mydomain`` from it. The default now uses ``ansible_facts['fqdn']``. Hosts without a resolvable domain still yield a short name, in which case ``myhostname`` should be set explicitly via ``run_postfix_maincf_settings``.
- ``run`` role - The documentation of the ``unmanaged`` service state falsely claimed the service "will not start at boot". The role leaves the service completely alone in this state: both the running state and the boot (enablement) state stay exactly as they are. The description now documents the real behavior.
- ``run`` role - The lookup table type descriptions no longer refer to a ``regex`` table type. Postfix calls it ``regexp``, which is also the value listed in the ``choices`` of the affected options.
- ``run`` role - The managed lookup-map references (``alias_maps``, ``recipient_canonical_maps``, ``sender_canonical_maps``, ``lmtp_generic_maps``, ``smtp_generic_maps``, ``relocated_maps``, ``transport_maps``, ``virtual_alias_maps``, ``relay_domains``, ``virtual_alias_domains`` and ``smtp_sasl_password_maps``) are no longer prepended to the corresponding main.cf setting when they are already present. The guards used an exact inequality (``!= managed``) instead of a containment check, so a value that already listed the role-managed map alongside another map (e.g. ``hash:/etc/aliases, lmdb:/other``) had the managed map prepended again, producing a duplicate entry. The guards (and the matching "is being extended" notices) now test list membership.
- ``run`` role - The nine feature flags (``run_postfix_relay_domains_manage``, ``run_postfix_access_manage``, ``run_postfix_aliases_manage``, ``run_postfix_canonical_manage``, ``run_postfix_generic_manage``, ``run_postfix_relocated_manage``, ``run_postfix_transport_manage``, ``run_postfix_virtual_manage``, ``run_postfix_smtp_sasl_password_manage``) now accept string booleans such as ``"true"`` (as supplied by key/value extra-vars, ``-e run_postfix_access_manage=true``). The feature gates used strict identity tests against the native boolean ``true``, so a string value silently skipped the requested feature - depending on the flag this omitted access controls, relay domains, lookup tables, aliases, transports or SMTP relay credentials without any error.
- ``run`` role - The optimization that skips re-gathering facts in ``tasks/init.yml`` never triggered. ``__run_postfix_used_facts`` was a flat list used both as the ``setup`` module's ``gather_subset`` and as a presence check against ``ansible_facts.keys()``. It contained the gather_subset name ``platform``, which is never a key in ``ansible_facts`` (that subset yields ``hostname``, ``fqdn``, …), so the check was always false and ``setup`` ran on every invocation. The variable is now a mapping of gather_subset name to the ``ansible_facts`` keys it provides, so each side gets the data it needs.
- ``run`` role - The service restart and reload handlers were gated only on ``run_postfix_service_state != 'unmanaged'``. With ``run_postfix_service_state: "disabled"`` a configuration change still notified them and, because handlers run after the service management tasks, the restart started the just-stopped unit again (and the reload failed on the inactive unit), leaving a running service although the declared state is stopped. The handlers are now gated on ``run_postfix_service_state in ['enabled', 'running']``.
- ``run`` role - ``run_postfix_state: absent`` only removed the packages and left every lookup table file the role had created on disk, contradicting the documented "reverts changes as much as possible". The uninstall now removes the files it created (``relay_domains``, ``access_recipient``, ``access_sender``, ``canonical_recipient``, ``canonical_sender``, ``generic``, ``relocated``, ``transport``, ``virtual``, ``virtual_domains`` and ``smtp_sasl_password``) together with any compiled database built from them. ``main.cf``, ``master.cf``, ``/etc/aliases`` and ``/etc/mailname`` are left untouched: the role only edits those in place and they belong to the package or the system.

Known Issues
------------

- ``run`` role - Aliases entries that an earlier version of this role disabled carry a plain ``#`` instead of the ``#ANSIBLE_DISABLED`` marker and can therefore not be told apart from entries disabled by the administrator. They are left untouched by ``run_postfix_state: absent`` and have to be re-enabled manually if wanted.

v1.3.0
======

Release Summary
---------------

Release Date: 2026-05-10

Maintenance and bugfix release.

Minor Changes
-------------

- Added Fedora 44 as supported platform for all collection roles and Molecule test scenarios.
- Added Ubuntu 26.04 LTS (Resolute Raccoon) as supported platform for all collection roles and Molecule test scenarios.

Removed Features (previously deprecated)
----------------------------------------

- Removed Fedora 42 support (End of Life, EOL) from collection roles and Molecule scenarios. The collection may still work on Fedora 42, but no testing or bugfixes will be provided. A warning will be displayed when used on unsupported platforms.

Bugfixes
--------

- ``foundata.postfix.run`` - Fixed uninstallation failure on Fedora 43/44 (and potentially other platforms in the future) caused by DNF dependency resolution errors. Map backend packages (e.g. ``postfix-pcre``) installed during setup were not included in the removal list, preventing the base ``postfix`` package from being removed. All map backend packages are now included in ``__run_postfix_packages_uninstall`` and removed together with the base package.

v1.2.0
======

Release Summary
---------------

Release Date: 2025-12-26

Maintenance release.

Minor Changes
-------------

- Added Fedora 43 as supported platform for all collection roles and Molecule test scenarios

Removed Features (previously deprecated)
----------------------------------------

- Removed Fedora 41 support (End of Life, EOL) from collection roles and Molecule scenarios. The collection may still work on Fedora 41, but no testing or bugfixes will be provided. A warning will be displayed when used on unsupported platforms.

v1.1.0
======

Release Summary
---------------

Release Date: 2025-11-02

Maintenance release.

Minor Changes
-------------

- Molecule: Added Debian 13 (Trixie) as a test target platform.
- Prevent duplicate alias declarations by commenting out previously existing aliases in the lookup-table when they are also defined in ``run_postfix_aliases_map`` or ``__run_postfix_aliases_map_defaults``.
- ``foundata.postfix.run``: Added Debian 13 (Trixie) as a supported platform.

Removed Features (previously deprecated)
----------------------------------------

- Molecule: Removed Debian 11 (Bullseye) as a test target platform.
- ``foundata.postfix.run``: Removed Debian 11 (Bullseye) from the list of supported platforms. The role will continue to work on Debian 11 but will display a warning. To avoid this, either remain on or pin the previous version of the collection. Bugs and issues related to Debian 11 will no longer be fixed.

v1.0.0
======

Release Summary
---------------

Release Date: 2025-05-30

First public release, providing all functionality and files.
