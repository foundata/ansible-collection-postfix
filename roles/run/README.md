# Ansible role: `foundata.postfix.run`

The `foundata.postfix.run` Ansible role (part if the `foundata.postfix` Ansible collection).

It provides automated configuration management of Postfix across major platforms. It does not explain how to create a good Postfix configuration that suits your requirements; that task remains for you to accomplish. However, the [examples playbooks](#examples) may help you a bit with that.



## Table of contents<a id="toc"></a>

- [Features](#features)<!-- ANSIBLE DOCSMITH TOC START -->
- [Role variables](#variables)
  - [`run_postfix_state`](#variable-run_postfix_state)
  - [`run_postfix_autoupgrade`](#variable-run_postfix_autoupgrade)
  - [`run_postfix_service_state`](#variable-run_postfix_service_state)
  - [`run_postfix_maincf_settings`](#variable-run_postfix_maincf_settings)
  - [`run_postfix_mastercf_settings`](#variable-run_postfix_mastercf_settings)
  - [`run_postfix_relay_domains_manage`](#variable-run_postfix_relay_domains_manage)
  - [`run_postfix_relay_domains_list_tabletype`](#variable-run_postfix_relay_domains_list_tabletype)
  - [`run_postfix_relay_domains_list`](#variable-run_postfix_relay_domains_list)
  - [`run_postfix_access_manage`](#variable-run_postfix_access_manage)
  - [`run_postfix_access_recipient_map_tabletype`](#variable-run_postfix_access_recipient_map_tabletype)
  - [`run_postfix_access_recipient_map`](#variable-run_postfix_access_recipient_map)
  - [`run_postfix_access_sender_map_tabletype`](#variable-run_postfix_access_sender_map_tabletype)
  - [`run_postfix_access_sender_map`](#variable-run_postfix_access_sender_map)
  - [`run_postfix_aliases_manage`](#variable-run_postfix_aliases_manage)
  - [`run_postfix_aliases_map_tabletype`](#variable-run_postfix_aliases_map_tabletype)
  - [`run_postfix_aliases_map`](#variable-run_postfix_aliases_map)
  - [`run_postfix_canonical_manage`](#variable-run_postfix_canonical_manage)
  - [`run_postfix_canonical_recipient_map_tabletype`](#variable-run_postfix_canonical_recipient_map_tabletype)
  - [`run_postfix_canonical_recipient_map`](#variable-run_postfix_canonical_recipient_map)
  - [`run_postfix_canonical_sender_map_tabletype`](#variable-run_postfix_canonical_sender_map_tabletype)
  - [`run_postfix_canonical_sender_map`](#variable-run_postfix_canonical_sender_map)
  - [`run_postfix_generic_manage`](#variable-run_postfix_generic_manage)
  - [`run_postfix_generic_map_tabletype`](#variable-run_postfix_generic_map_tabletype)
  - [`run_postfix_generic_map`](#variable-run_postfix_generic_map)
  - [`run_postfix_relocated_manage`](#variable-run_postfix_relocated_manage)
  - [`run_postfix_relocated_map_tabletype`](#variable-run_postfix_relocated_map_tabletype)
  - [`run_postfix_relocated_map`](#variable-run_postfix_relocated_map)
  - [`run_postfix_transport_manage`](#variable-run_postfix_transport_manage)
  - [`run_postfix_transport_map_tabletype`](#variable-run_postfix_transport_map_tabletype)
  - [`run_postfix_transport_map`](#variable-run_postfix_transport_map)
  - [`run_postfix_virtual_manage`](#variable-run_postfix_virtual_manage)
  - [`run_postfix_virtual_aliasdomains_list_tabletype`](#variable-run_postfix_virtual_aliasdomains_list_tabletype)
  - [`run_postfix_virtual_aliasdomains_list`](#variable-run_postfix_virtual_aliasdomains_list)
  - [`run_postfix_virtual_alias_map_tabletype`](#variable-run_postfix_virtual_alias_map_tabletype)
  - [`run_postfix_virtual_alias_map`](#variable-run_postfix_virtual_alias_map)
  - [`run_postfix_smtp_sasl_password_manage`](#variable-run_postfix_smtp_sasl_password_manage)
  - [`run_postfix_smtp_sasl_password_map_tabletype`](#variable-run_postfix_smtp_sasl_password_map_tabletype)
  - [`run_postfix_smtp_sasl_password_map`](#variable-run_postfix_smtp_sasl_password_map)
<!-- ANSIBLE DOCSMITH TOC END -->
- [Example playbooks, using this role](#examples)
- [Supported tags](#tags)
- [Dependencies](#dependencies)
- [Compatibility](#compatibility)
- [External requirements](#requirements)



## Features<a id="features"></a>

Main features:

* Full support for managing all `main.cf` and `master.cf` settings.
* Automatic detection and installation of required backend packages for map types (e.g., LMDB, MySQL, PostgreSQL) based on your configuration.
* Specialized parameters for simplified management of commonly needed Postfix settings, such as:
  * Lookup tables for `virtual`, `transport`, and other mappings.
  * Management of `relay_domains` for fine-grained control over relaying behavior.
* Provides sensible, production-ready defaults, tuned for recent Postfix versions, including support for modern map types like LMDB.
* Designed for cross-platform compatibility, working seamlessly across major Linux distributions.


<!-- ANSIBLE DOCSMITH MAIN START -->

## Role variables<a id="variables"></a>

The following variables can be configured for this role:

| Variable | Type | Required | Default | Description (abstract) |
|----------|------|----------|---------|------------------------|
| `run_postfix_state` | `str` | No | `"presents"` | Determines whether the managed resources should be `present` or `absent`.<br><br>`present` ensures that required components, such as software packages, are installed and configured.<br><br>`absent` reverts changes as much as possible, such as […](#variable-run_postfix_state) |
| `run_postfix_autoupgrade` | `bool` | No | `false` | If set to `true`, all managed packages will be upgraded during each Ansible run (e.g., when the package provider detects a newer version than the currently installed one). |
| `run_postfix_service_state` | `str` | No | `"enabled"` | Defines the status of the service(s). Possible values:<br><br>- `enabled`: Service is running and will start automatically at boot. - `disabled`: Service is stopped and will not start automatically at boot. - `running``: Service is running but will […](#variable-run_postfix_service_state) |
| `run_postfix_maincf_settings` | `dict` | No | `{}` | Additional configuration for Postfix daemon (additional config values or to overwrite defaults from `__run_postfix_maincf_settings_defaults` in `vars/main.yml`).<br><br>Use standard Postfix option names as keys with their corresponding […](#variable-run_postfix_maincf_settings) |
| `run_postfix_mastercf_settings` | `dict` | No | `{}` | Additional configuration for Postfix master process (`master.cf``). For details on the syntax of the fields/values, see the master(5) manual page (command: `man 5 master` or online: http://www.postfix.org/master.5.html).<br><br>Dictionary […](#variable-run_postfix_mastercf_settings) |
| `run_postfix_relay_domains_manage` | `bool` | No | `false` | Switch to control if relay domains settings are handled by this role.<br><br>When set to `true` (which is the default), the role will:<br><br>1. Create and manage a config file for relay domains using the `run_postfix_relay_domains_list` variable for […](#variable-run_postfix_relay_domains_manage) |
| `run_postfix_relay_domains_list_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_relay_domains_list` Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best […](#variable-run_postfix_relay_domains_list_tabletype) |
| `run_postfix_relay_domains_list` | `list` | No | `[]` | Relay domains for Postfix. This list of relay domains for which Postfix will accept mail for relaying to their final destinations.<br><br>Example:<br><br>``` - "example.com" - "example.net" - "example.org" - "foo.example.org" ```<br><br>Will be […](#variable-run_postfix_relay_domains_list) |
| `run_postfix_access_manage` | `bool` | No | `false` | Switch to control whether the lookup table `access` is controlled by dedicated settings and tasks of this role.<br><br>More information: - https://www.postfix.org/access.5.html - https://www.postfix.org/SMTPD_ACCESS_README.html - […](#variable-run_postfix_access_manage) |
| `run_postfix_access_recipient_map_tabletype` | `str` | No | `"pcre"` | The lookup table type to use if for the table defined by `run_postfix_access_recipient_map`.<br><br>Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the […](#variable-run_postfix_access_recipient_map_tabletype) |
| `run_postfix_access_recipient_map` | `dict` | No | `{}` | Lookup-table `access`: Recipient address restriction rules.<br><br>Examples:<br><br>``` "abuse@example.com": "OK" # Whitelist RFC 2142 address "postmaster@example.com": "OK" # Whitelist RFC 2142 address "burned@example.com": "REJECT" # User is gone, […](#variable-run_postfix_access_recipient_map) |
| `run_postfix_access_sender_map_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the table defined by `run_postfix_access_sender_map`.<br><br>Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best […](#variable-run_postfix_access_sender_map_tabletype) |
| `run_postfix_access_sender_map` | `dict` | No | `{}` | Lookup-table `access`: Sender address restriction rules.<br><br>More information: - https://www.postfix.org/access.5.html - https://en.wikipedia.org/wiki/List_of_SMTP_server_return_codes<br><br>Examples:<br><br>``` "@microsoft.com": "500 Deal with […](#variable-run_postfix_access_sender_map) |
| `run_postfix_aliases_manage` | `bool` | No | `false` | Switch to control whether the lookup table `aliases` is controlled by dedicated settings and tasks of this role.<br><br>`aliases` can rewrite local(!) addresses (no domain part) and redirect mails to other mailboxes, files or commands: - […](#variable-run_postfix_aliases_manage) |
| `run_postfix_aliases_map_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_aliases_map` Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best default. […](#variable-run_postfix_aliases_map_tabletype) |
| `run_postfix_aliases_map` | `dict` | No | `{}` | Lookup-table `alias`: address mappings.<br><br>More information: - https://www.postfix.org/access.5.html - https://en.wikipedia.org/wiki/List_of_SMTP_server_return_codes<br><br>Examples:<br><br>``` abuse: "root" deamon123: "root, tux" foo: […](#variable-run_postfix_aliases_map) |
| `run_postfix_canonical_manage` | `bool` | No | `false` | Switch to control whether the lookup table `canonical` is controlled by dedicated settings and tasks of this role.<br><br>`canonical` can rewrite mail addresses before mail is stored into the queue: - https://www.postfix.org/canonical.5.html - […](#variable-run_postfix_canonical_manage) |
| `run_postfix_canonical_recipient_map_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_canonical_recipient_map` Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best […](#variable-run_postfix_canonical_recipient_map_tabletype) |
| `run_postfix_canonical_recipient_map` | `dict` | No | `{}` | Lookup-table `canonical`: Recipient address mappings / rewrite rules.<br><br>Examples:<br><br>``` "old.name@example.com": "new.name@example.com" "root": "user123@example.com" "@example.com": "@example.net" ```<br><br>Will be ignored if […](#variable-run_postfix_canonical_recipient_map) |
| `run_postfix_canonical_sender_map_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_canonical_sender_map` Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best […](#variable-run_postfix_canonical_sender_map_tabletype) |
| `run_postfix_canonical_sender_map` | `dict` | No | `{}` | Lookup-table `canonical`: Sender address mappings / rewrite rules.<br><br>Examples:<br><br>``` "old.name@example.com": "new.name@example.com" "root": "user123@example.com" "/.+/": "do-not-reply@example.com" # rewrite all outgoing mail addresses […](#variable-run_postfix_canonical_sender_map) |
| `run_postfix_generic_manage` | `bool` | No | `false` | Switch to control whether the lookup table `generic` is controlled by dedicated settings and tasks of this role.<br><br>`generic` is used to rewrite sender addresses in emails leaving the system via SMTP/LMTP only: - […](#variable-run_postfix_generic_manage) |
| `run_postfix_generic_map_tabletype` | `str` | No | `"pcre"` | The lookup table type to use if for the list defined by `run_postfix_generic_map`<br><br>Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best […](#variable-run_postfix_generic_map_tabletype) |
| `run_postfix_generic_map` | `dict` | No | `{}` | Lookup-table 'generic': Sender address mappings / rewrite rules.<br><br>Examples:<br><br>``` "john.doe@example.local": "john.doe@example.com" "@example.local": "contact@example.net" ```<br><br>Will be ignored if `run_postfix_generic_manage` is set to […](#variable-run_postfix_generic_map) |
| `run_postfix_relocated_manage` | `bool` | No | `false` | Switch to control whether the lookup table `relocated` is controlled by dedicated settings and tasks of this role.<br><br>`relocated` is used to tell senders that a recipient's address has changed. If someone tries to send email to an old address, […](#variable-run_postfix_relocated_manage) |
| `run_postfix_relocated_map_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_relocated_map`<br><br>Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best […](#variable-run_postfix_relocated_map_tabletype) |
| `run_postfix_relocated_map` | `dict` | No | `{}` | Lookup-table `relocated`: old-address new-address-info mappings / rules.<br><br>Dictionary structure:<br><br>- Keys: Criterions / pattern to describe old addresses. - Values: The new location, usually email addresses (or phone or other address or […](#variable-run_postfix_relocated_map) |
| `run_postfix_transport_manage` | `bool` | No | `false` | Switch to control whether the lookup table `transport` is controlled by dedicated settings and tasks of this role.<br><br>`transport` defines the mail transport routes for Postfix, mapping domains or email addresses to specific mail delivery methods […](#variable-run_postfix_transport_manage) |
| `run_postfix_transport_map_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_transport_map`<br><br>Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best […](#variable-run_postfix_transport_map_tabletype) |
| `run_postfix_transport_map` | `dict` | No | `{}` | Domain transport mapping for Postfix. This defines where to send emails to for the listed domains in a structured format.<br><br>Dictionary structure:<br><br>- Keys: Criterions / pattern to describe domains. - Values: Lists of destinations, usually […](#variable-run_postfix_transport_map) |
| `run_postfix_virtual_manage` | `bool` | No | `false` | Switch to control whether the lookup table "virtual" is controlled by dedicated settings and tasks of this role.<br><br>"virtual" defines the address mappings used by Postfix to redirect email addresses in virtual alias domains to other local or […](#variable-run_postfix_virtual_manage) |
| `run_postfix_virtual_aliasdomains_list_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_virtual_aliasdomains_list`<br><br>Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be […](#variable-run_postfix_virtual_aliasdomains_list_tabletype) |
| `run_postfix_virtual_aliasdomains_list` | `list` | No | `[]` | Virtual alias domains for Postfix. This tells Postfix to accept incoming emails for a list of virtual domains. Mappings for them are defined via `run_postfix_virtual_alias_map`.<br><br>``` Example: - "example.com" - "example.net" ```<br><br>Will be […](#variable-run_postfix_virtual_aliasdomains_list) |
| `run_postfix_virtual_alias_map_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_virtual_alias_map`<br><br>Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best […](#variable-run_postfix_virtual_alias_map_tabletype) |
| `run_postfix_virtual_alias_map` | `dict` | No | `{}` | Virtual alias mappings for Postfix. This defines email forwarding rules in a structured format.<br><br>Dictionary structure:<br><br>- Keys: Criterions / pattern to describe the virtual address(es) Attention: Do not forget to list handled domains in […](#variable-run_postfix_virtual_alias_map) |
| `run_postfix_smtp_sasl_password_manage` | `bool` | No | `false` | Switch to control whether SMTP SASL password credentials are controlled by dedicated settings and tasks of this role.<br><br>They are used to authenticate Postfix when sending emails through a relay server that requires authentication: - […](#variable-run_postfix_smtp_sasl_password_manage) |
| `run_postfix_smtp_sasl_password_map_tabletype` | `str` | No | `"lmdb"` | The lookup table type to use if for the list defined by `run_postfix_smtp_sasl_password_map`<br><br>Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the […](#variable-run_postfix_smtp_sasl_password_map_tabletype) |
| `run_postfix_smtp_sasl_password_map` | `dict` | No | `{}` | SMTP SASL password credentials for Postfix. Each entry maps a remote SMTP server (and optionally a port) to a username and password.<br><br>Dictionary structure:<br><br>- Keys: Criterions, the destination to specify credentials for. Attention: If you […](#variable-run_postfix_smtp_sasl_password_map) |

### `run_postfix_state`<a id="variable-run_postfix_state"></a>

[*⇑ Back to ToC ⇑*](#toc)

Determines whether the managed resources should be `present` or `absent`.

`present` ensures that required components, such as software packages, are
installed and configured.

`absent` reverts changes as much as possible, such as removing packages,
deleting created users, stopping services, restoring modified settings, …

- **Type**: `str`
- **Required**: No
- **Default**: `"presents"`
- **Choices**: `present`, `absent`



### `run_postfix_autoupgrade`<a id="variable-run_postfix_autoupgrade"></a>

[*⇑ Back to ToC ⇑*](#toc)

If set to `true`, all managed packages will be upgraded during each Ansible
run (e.g., when the package provider detects a newer version than the
currently installed one).

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_service_state`<a id="variable-run_postfix_service_state"></a>

[*⇑ Back to ToC ⇑*](#toc)

Defines the status of the service(s). Possible values:

- `enabled`: Service is running and will start automatically at boot.
- `disabled`: Service is stopped and will not start automatically at boot.
- `running``: Service is running but will not start automatically at boot.
  This can be used to start a service on the first Ansible run without
  enabling it for boot.
- `unmanaged`: Service will not start at boot, and Ansible will not
  manage its running state. This is primarily useful when services are
  monitored and managed by systems other than Ansible.

The singular form ("service") is used for simplicity. However, the defined
status applies to all services if multiple are being managed by this role.

- **Type**: `str`
- **Required**: No
- **Default**: `"enabled"`
- **Choices**: `enabled`, `disabled`, `running`, `unmanaged`



### `run_postfix_maincf_settings`<a id="variable-run_postfix_maincf_settings"></a>

[*⇑ Back to ToC ⇑*](#toc)

Additional configuration for Postfix daemon (additional config values or to
overwrite defaults from `__run_postfix_maincf_settings_defaults` in
`vars/main.yml`).

Use standard Postfix option names as keys with their corresponding values.

Dictionary structure:

- Keys: Standard Postfix option names (as defined in postconf documentation)
- Values: Corresponding configuration values. Common options include:
  - `myhostname`: FQDN of the mail system
  - `mydomain`: Domain part of the FQDN
  - `myorigin`: Domain to append to locally posted mail
  - `inet_interfaces`: Network interfaces to listen on
  - `inet_protocols`: IP protocols to use (ipv4, ipv6, all)
  - `mynetworks`: Trusted SMTP clients for relay
  - If a value is a list (which can be whitespace or comma-separated in
    Postfix), it can also be passed as YAML list and will be converted into a
    comma-separated value string automatically. So all three variants are valid
    here to define a list as value:
    ```
    mynetworks:
    - "127.0.0.0/8"
    - "[::ffff:127.0.0.0]/104"
    - "[::1]/128"
    mynetworks: "127.0.0.0/8, [::ffff:127.0.0.0]/104, [::1]/128"
    mynetworks: "127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128"
    ```

Reference commands:

- `postconf``: print all current active values
- `postconf -n`: print all active, explicitly set options
- `postconf -d`: print all options with their default

Please note that you do not need own handler for postmap when using
`hash|btree|dbm:`` somewhere. The role's handler is intelligent enough to scan
the config for hash|btree|dbm:/path values. Just notify
`run_postfix: update lookup tables` on changes when writing additional, own
tasks.

For a complete list of options, see https://www.postfix.org/postconf.5.html

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_mastercf_settings`<a id="variable-run_postfix_mastercf_settings"></a>

[*⇑ Back to ToC ⇑*](#toc)

Additional configuration for Postfix master process (`master.cf``). For
details on the syntax of the fields/values, see the master(5) manual page
(command: `man 5 master` or online: http://www.postfix.org/master.5.html).

Dictionary structure:

- Keys: service names (e.g., smtp, submission, 127.0.0.1:smtp, 12345)
- Values: configuration parameters for that service

Example:

```
smtp: # Service for standard SMTP (Port 25)
  type: "inet" # Network socket type
  private: "n" # Private service
  unpriv: "-" # Unprivileged service
  chroot: "n" # Run in chroot, usually n for Postfix > v3.0.0
  wakeup: "-" # Wakeup time
  maxproc: "-" # Max processes
  command_args: "smtpd" # Command and arguments
submission: # Service for submission (Port 587)
  type: "inet"
  private: "n"
  unpriv: "-"
  chroot: "n"
  wakeup: "-"
  maxproc: "-"
  command_args: "smtpd"
```

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_relay_domains_manage`<a id="variable-run_postfix_relay_domains_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control if relay domains settings are handled by this role.

When set to `true` (which is the default), the role will:

1. Create and manage a config file for relay domains using the
   `run_postfix_relay_domains_list` variable for the contents
2. Set (or override) `main.cf` settings for the relay_domains option
   accordingly.

When `false`, you'll need to configure these settings manually through
`run_postfix_maincf_settings` or through other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_relay_domains_list_tabletype`<a id="variable-run_postfix_relay_domains_list_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by `run_postfix_relay_domains_list`
Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best default. "hash" and "btree" are available on systems with support for Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster than `regex`.
Will be ignored if `run_postfix_relay_domains_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_relay_domains_list`<a id="variable-run_postfix_relay_domains_list"></a>

[*⇑ Back to ToC ⇑*](#toc)

Relay domains for Postfix. This list of relay domains for which Postfix will
accept mail for relaying to their final destinations.

Example:

```
- "example.com"
- "example.net"
- "example.org"
- "foo.example.org"
```

Will be ignored if `run_postfix_relay_domains_manage` is set to `false`.

- **Type**: `list`
- **Required**: No
- **Default**: `[]`
- **List Elements**: `str`



### `run_postfix_access_manage`<a id="variable-run_postfix_access_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control whether the lookup table `access` is controlled by
dedicated settings and tasks of this role.

More information:
- https://www.postfix.org/access.5.html
- https://www.postfix.org/SMTPD_ACCESS_README.html
- https://en.wikipedia.org/wiki/List_of_SMTP_server_return_codes

When set to `true` (which is the default), the role will:

1. Create and manage table file(s) to manage mappings. It uses variables
   for the contents:
   - `run_postfix_access_recipient_map`
   - `run_postfix_access_sender_map`
2. Set or change `main.cf` settings accordingly:
   - `smtpd_relay_restrictions:` The role will introduce new `smtpd_restriction_classes`:
      - `recipient_access: {{ run_postfix_access_recipient_map_tabletype }}:/etc/postfix/access-sender`
      - `sender_access: {{ run_postfix_access_sender_map_tabletype }}:/etc/postfix/access-recipient`
      and add them at before all other `smtpd_relay_restrictions` rules.

When `false`, you'll need to configure these settings or functionality manually
through `run_postfix_maincf_settings` or other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_access_recipient_map_tabletype`<a id="variable-run_postfix_access_recipient_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the table defined by
`run_postfix_access_recipient_map`.

Most distributions and builds dropped support for Berkeley DB with
Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be
the best default. `hash` and `btree` are available on systems with support for
Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster
than `regex`.

Will be ignored if `run_postfix_access_manage` is set to `false``.

- **Type**: `str`
- **Required**: No
- **Default**: `"pcre"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_access_recipient_map`<a id="variable-run_postfix_access_recipient_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Lookup-table `access`: Recipient address restriction rules.

Examples:

```
"abuse@example.com": "OK" # Whitelist RFC 2142 address
"postmaster@example.com": "OK" # Whitelist RFC 2142 address
"burned@example.com": "REJECT" # User is gone, address get mass of spam
```

Will be ignored if `run_postfix_access_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_access_sender_map_tabletype`<a id="variable-run_postfix_access_sender_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the table defined by
`run_postfix_access_sender_map`.

Most distributions and builds dropped support for Berkeley DB with
Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be
the best default. `hash` and `btree` are available on systems with support for
Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster
than `regex`.

Will be ignored if `run_postfix_access_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_access_sender_map`<a id="variable-run_postfix_access_sender_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Lookup-table `access`: Sender address restriction rules.

More information:
- https://www.postfix.org/access.5.html
- https://en.wikipedia.org/wiki/List_of_SMTP_server_return_codes

Examples:

```
"@microsoft.com": "500 Deal with it"
"spammy@example.com": "REJECT"
"spammy2@example.com": "DISCARD" # Attention: silent drop
"important@example.net": "OK"
```

Will be ignored if `run_postfix_access_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_aliases_manage`<a id="variable-run_postfix_aliases_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control whether the lookup table `aliases` is controlled by
dedicated settings and tasks of this role.

`aliases` can rewrite local(!) addresses (no domain part) and redirect mails
to other mailboxes, files or commands:
- https://www.postfix.org/aliases.5.html
- https://www.postfix.org/local.8.html
- https://www.postfix.org/postconf.5.html#alias_database
- https://www.postfix.org/postconf.5.html#alias_maps

When set to `true` (which is the default), the role will:

1. Create and manage table file(s) to manage mappings. It uses variables
   for the contents:
   - `run_postfix_aliases_map`
2. Set (or extend) main.cf settings accordingly:
   - `alias_maps`

When `false`, you'll need to configure these settings or functionality manually
through `run_postfix_maincf_settings` or other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_aliases_map_tabletype`<a id="variable-run_postfix_aliases_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by `run_postfix_aliases_map`
Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best default. "hash" and "btree" are available on systems with support for Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster than `regex`.
Will be ignored if `run_postfix_aliases_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_aliases_map`<a id="variable-run_postfix_aliases_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Lookup-table `alias`: address mappings.

More information:
- https://www.postfix.org/access.5.html
- https://en.wikipedia.org/wiki/List_of_SMTP_server_return_codes

Examples:

```
abuse: "root"
deamon123: "root, tux"
foo: "/var/spool/mail/foo"
bar: "| my_mail_script.sh"
baz: "/dev/null"
tux: "tux@example.com"
root: "admin-info@example.com, admin2@example.net"
```

Will be ignored if `run_postfix_aliases_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_canonical_manage`<a id="variable-run_postfix_canonical_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control whether the lookup table `canonical` is controlled by
dedicated settings and tasks of this role.

`canonical` can rewrite mail addresses before mail is stored into the queue:
- https://www.postfix.org/canonical.5.html
- https://www.postfix.org/postconf.5.html#sender_canonical_maps

When set to `true` (which is the default), the role will:

1. Create and manage table file(s) to manage mappings. It uses variables
   for the contents:
   - `run_postfix_canonical_recipient_map`
   - `run_postfix_canonical_sender_map`
2. Set (or extend) main.cf settings accordingly:
   - `sender_canonical_maps`
   - `recipient_canonical_maps`

When `false`, you'll need to configure these settings or functionality manually
through `run_postfix_maincf_settings` or other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_canonical_recipient_map_tabletype`<a id="variable-run_postfix_canonical_recipient_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by `run_postfix_canonical_recipient_map`
Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best default. "hash" and "btree" are available on systems with support for Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster than `regex`.
Will be ignored if `run_postfix_canonical_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_canonical_recipient_map`<a id="variable-run_postfix_canonical_recipient_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Lookup-table `canonical`: Recipient address mappings / rewrite rules.

Examples:

```
"old.name@example.com": "new.name@example.com"
"root": "user123@example.com"
"@example.com": "@example.net"
```

Will be ignored if `run_postfix_canonical_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_canonical_sender_map_tabletype`<a id="variable-run_postfix_canonical_sender_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by `run_postfix_canonical_sender_map`
Most distributions and builds dropped support for Berkeley DB with Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be the best default. "hash" and "btree" are available on systems with support for Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster than `regex`.
Will be ignored if `run_postfix_canonical_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_canonical_sender_map`<a id="variable-run_postfix_canonical_sender_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Lookup-table `canonical`: Sender address mappings / rewrite rules.

Examples:

```
"old.name@example.com": "new.name@example.com"
"root": "user123@example.com"
"/.+/": "do-not-reply@example.com" # rewrite all outgoing mail addresses
"@example.com": "@example.net"
```

Will be ignored if `run_postfix_canonical_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_generic_manage`<a id="variable-run_postfix_generic_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control whether the lookup table `generic` is controlled by
dedicated settings and tasks of this role.

`generic` is used to rewrite sender addresses in emails leaving the system
via SMTP/LMTP only:
- https://www.postfix.org/generic.5.html
- https://www.postfix.org/postconf.5.html#lmtp_generic_maps
- https://www.postfix.org/postconf.5.html#smtp_generic_maps

When set to `true` (which is the default), the role will:

1. Create and manage table file(s) to manage mappings. It uses variables
   for the contents:
   - `run_postfix_generic_map`
2. Set (or extend) main.cf settings accordingly:
   - `lmtp_generic_maps`
   - `smtp_generic_maps`

When `false`, you'll need to configure these settings or functionality manually
through `run_postfix_maincf_settings` or other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_generic_map_tabletype`<a id="variable-run_postfix_generic_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by
`run_postfix_generic_map`

Most distributions and builds dropped support for Berkeley DB with
Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be
the best default. "hash" and "btree" are available on systems with support for
Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster
than `regex`.

Will be ignored if `run_postfix_generic_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"pcre"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_generic_map`<a id="variable-run_postfix_generic_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Lookup-table 'generic': Sender address mappings / rewrite rules.

Examples:

```
"john.doe@example.local": "john.doe@example.com"
"@example.local": "contact@example.net"
```

Will be ignored if `run_postfix_generic_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_relocated_manage`<a id="variable-run_postfix_relocated_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control whether the lookup table `relocated` is controlled by
dedicated settings and tasks of this role.

`relocated` is used to tell senders that a recipient's address has changed.
If someone tries to send email to an old address, Postfix generates a
non-delivery report (NDR) with the new address ("user has moved to
<new_location>" bounce messages):
- https://www.postfix.org/relocated.5.html
- https://www.postfix.org/postconf.5.html#relocated_maps

When set to `true` (which is the default), the role will:

1. Create and manage table file(s) to manage mappings. It uses variables
   for the contents:
   - `run_postfix_relocated_map`
2. Set (or extend) main.cf settings accordingly:
   - `relocated_maps`

When `false`, you'll need to configure these settings or functionality manually
through `run_postfix_maincf_settings` or other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_relocated_map_tabletype`<a id="variable-run_postfix_relocated_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by
`run_postfix_relocated_map`

Most distributions and builds dropped support for Berkeley DB with
Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be
the best default. "hash" and "btree" are available on systems with support for
Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster
than `regex`.

Will be ignored if `run_postfix_relocated_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_relocated_map`<a id="variable-run_postfix_relocated_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Lookup-table `relocated`: old-address new-address-info mappings / rules.

Dictionary structure:

- Keys: Criterions / pattern to describe old addresses.
- Values: The new location, usually email addresses (or phone or other
          address or location strings; Postfix's default template starts with
          "User has moved to " so use a fitting string).
Example:

```
"user@example.com": "new@example.net"
"@example.com": "Sorry, all mail has to be sent to example.net"
```

Will be ignored if `run_postfix_relocated_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_transport_manage`<a id="variable-run_postfix_transport_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control whether the lookup table `transport` is controlled by
dedicated settings and tasks of this role.

`transport` defines the mail transport routes for Postfix, mapping domains
or email addresses to specific mail delivery methods and destinations:
- https://www.postfix.org/transport.5.html
- https://www.postfix.org/postconf.5.html#transport_maps

When set to `true` (which is the default), the role will:

1. Create and manage table file(s) to manage mappings. It uses variables
   for the contents:
   - `run_postfix_transport_map`
2. Set (or extend) main.cf settings accordingly:
   - `transport_maps`

When `false`, you'll need to configure these settings or functionality manually
through `run_postfix_maincf_settings` or other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_transport_map_tabletype`<a id="variable-run_postfix_transport_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by
`run_postfix_transport_map`

Most distributions and builds dropped support for Berkeley DB with
Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be
the best default. "hash" and "btree" are available on systems with support for
Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster
than `regex`.

Will be ignored if `run_postfix_transport_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_transport_map`<a id="variable-run_postfix_transport_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Domain transport mapping for Postfix. This defines where to send emails to
for the listed domains in a structured format.

Dictionary structure:

- Keys: Criterions / pattern to describe domains.
- Values: Lists of destinations, usually email servers or relays.
  Attention: Do not forget to use square brackets to force DNS A record
             instead of MX record lookups.
Examples:

```
"example.com": "lmtp:unix:/var/spool/postfix/private/lmtp-dovecot"
"example.net": "lmtp:127.0.0.1:24"
"example.org": "[192.168.0.1]"
"foo.example.org": "[mailrelay.foo.example.org]:123"
# dedicated relay for Microsoft
"/.*@(outlook|hotmail|live|msn)\..*/i": "[relay.example.com]:587"
```

Will be ignored if `run_postfix_transport_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_virtual_manage`<a id="variable-run_postfix_virtual_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control whether the lookup table "virtual" is controlled by
dedicated settings and tasks of this role.

"virtual" defines the address mappings used by Postfix to redirect email
addresses in virtual alias domains to other local or remote addresses:

- http://www.postfix.org/VIRTUAL_README.html
- https://www.postfix.org/virtual.5.html
- https://www.postfix.org/postconf.5.html#virtual_alias_domains
- https://www.postfix.org/postconf.5.html#virtual_alias_maps
- https://serverfault.com/questions/644306/confused-about-alias-maps-and-virtual-alias-maps

When set to `true` (which is the default), the role will:

1. Create and manage table file(s) to manage mappings. It uses variables
   for the contents:
   - `run_postfix_virtual_aliasdomains_list`
   - `run_postfix_virtual_alias_map`
2. Set (or override or extend) main.cf settings accordingly:
   - `virtual_alias_domains`
   - `virtual_alias_maps`

When `false`, you'll need to configure these settings or functionality manually
through `run_postfix_maincf_settings` or other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_virtual_aliasdomains_list_tabletype`<a id="variable-run_postfix_virtual_aliasdomains_list_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by
`run_postfix_virtual_aliasdomains_list`

Most distributions and builds dropped support for Berkeley DB with
Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be
the best default. "hash" and "btree" are available on systems with support for
Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster
than `regex`.

Will be ignored if `run_postfix_virtual_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_virtual_aliasdomains_list`<a id="variable-run_postfix_virtual_aliasdomains_list"></a>

[*⇑ Back to ToC ⇑*](#toc)

Virtual alias domains for Postfix. This tells Postfix to accept incoming
emails for a list of virtual domains. Mappings for them are defined via
`run_postfix_virtual_alias_map`.

```
Example:
  - "example.com"
  - "example.net"
```

Will be ignored if `run_postfix_virtual_manage` is set to `false`.

- **Type**: `list`
- **Required**: No
- **Default**: `[]`
- **List Elements**: `str`



### `run_postfix_virtual_alias_map_tabletype`<a id="variable-run_postfix_virtual_alias_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by
`run_postfix_virtual_alias_map`

Most distributions and builds dropped support for Berkeley DB with
Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be
the best default. "hash" and "btree" are available on systems with support for
Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster
than `regex`.

Will be ignored if `run_postfix_virtual_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_virtual_alias_map`<a id="variable-run_postfix_virtual_alias_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

Virtual alias mappings for Postfix. This defines email forwarding rules in a
structured format.

Dictionary structure:

- Keys: Criterions / pattern to describe the virtual address(es)
  Attention: Do not forget to list handled domains in
             `run_postfix_virtual_aliasdomains_list`
- Values: Lists of destinations, usually email addresses

Example:

```
"admin@example.com":
  - "admin@actual-destination.example.com"
"distribution@example.com":
  - "user1@actual-destination.example.com"
  - "user2@actual-destination.example.com"
  - "user3@actual-destination.example.com"
  - "user3@actual-destination.example.com"
"@example.com": # catch-all for example.com
  - "office@example.com"
"@example.net": # catch-all for example.net, rewrite all to @example.com
  - "example.com"
```

Will be ignored if `run_postfix_virtual_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`



### `run_postfix_smtp_sasl_password_manage`<a id="variable-run_postfix_smtp_sasl_password_manage"></a>

[*⇑ Back to ToC ⇑*](#toc)

Switch to control whether SMTP SASL password credentials are controlled by
dedicated settings and tasks of this role.

They are used to authenticate Postfix when sending emails through a relay
server that requires authentication:
- https://www.postfix.org/SASL_README.html#client_sasl
- https://www.postfix.org/postconf.5.html#smtp_sasl_password_maps

When set to `true` (which is the default), the role will:

1. Create and manage table file(s) to manage mappings. It uses variables
   for the contents:
   - `run_postfix_smtp_sasl_password_map`
2. Set (or extend) main.cf settings accordingly:
   - `smtp_sasl_password_maps`

When `false`, you'll need to configure these settings or functionality manually
through `run_postfix_maincf_settings` or other means.

- **Type**: `bool`
- **Required**: No
- **Default**: `false`



### `run_postfix_smtp_sasl_password_map_tabletype`<a id="variable-run_postfix_smtp_sasl_password_map_tabletype"></a>

[*⇑ Back to ToC ⇑*](#toc)

The lookup table type to use if for the list defined by
`run_postfix_smtp_sasl_password_map`

Most distributions and builds dropped support for Berkeley DB with
Postfix ≥ 3.9 and switched to `lmdb` (OpenLDAP LMDB database) which should be
the best default. "hash" and "btree" are available on systems with support for
Berkeley DB (and therefore deprecated / legacy now). `pcre` is usually faster
than `regex`.

Will be ignored if `run_postfix_smtp_sasl_password_manage` is set to `false`.

- **Type**: `str`
- **Required**: No
- **Default**: `"lmdb"`
- **Choices**: `btree`, `cdb`, `cidr`, `dbm`, `environ`, `fail`, `hash`, `inline`, `internal`, `lmdb`, `ldap`, `memcache`, `mongodb`, `mysql`, `netinfo`, `nis`, `nisplus`, `pcre`, `pipemap`, `pgsql`, `proxy`, `randmap`, `regexp`, `sdbm`, `socketmap`, `sqlite`, `static`, `tcp`, `texthash`, `unionmap`, `unix`



### `run_postfix_smtp_sasl_password_map`<a id="variable-run_postfix_smtp_sasl_password_map"></a>

[*⇑ Back to ToC ⇑*](#toc)

SMTP SASL password credentials for Postfix. Each entry maps a remote SMTP
server (and optionally a port) to a username and password.

Dictionary structure:

- Keys: Criterions, the destination to specify credentials for.
  Attention: If you specify the `[` and `]` where the relayhost destination
             is defined, then you must use the same form here.
- Values: The credentials in the `username:password` format.
  Note: The main.cf setting `smtp_sasl_password_result_delimiter` (which
  defaults to `:`) can be used to specify an alternative separator between
  username and password.

Example:

```
"[relay.example.com]": "user:password_superSecret-c0e-400a-b683-7ae5"
"[relay.example.com]:587": "johndoe:password-69f7d34-4c-123"
```

will be ignored if `run_postfix_smtp_sasl_password_manage` is set to `false`.

- **Type**: `dict`
- **Required**: No
- **Default**: `{}`




<!-- ANSIBLE DOCSMITH MAIN END -->


## Example playbooks, using this role<a id="examples"></a>

Installation with automatic upgrade:

```yaml
---

- name: "Initialize the foundata.postfix.run role"
  hosts: localhost
  gather_facts: false
  tasks:

    - name: "Trigger invocation of the foundata.postfix.run role"
      ansible.builtin.include_role:
        name: "foundata.postfix.run"
      vars:
        run_postfix_autoupgrade: true
```

Install and do some basic and common configuration:

```yaml
---

- name: "Initialize the foundata.postfix.run role"
  hosts: localhost
  gather_facts: false
  tasks:

    - name: "Trigger invocation of the foundata.postfix.run role"
      ansible.builtin.include_role:
        name: "foundata.postfix.run"
      vars:
        run_postfix_autoupgrade: true
        run_postfix_maincf_settings:
          myhostname: "smtp.example.com"
          mydomain: "example.com"
          inet_interfaces: "all"
          inet_protocols: "all"
          smtp_tls_security_level: "may"
          smtpd_tls_security_level: "may"
          smtp_tls_note_starttls_offer: "yes"
          smtpd_tls_cert_file: "/etc/ssl/certs/smtp.example.com/fullchain.cer"
          smtpd_tls_key_file: "/etc/ssl/private/smtp.example.com/cert.key"
          smtpd_tls_dh1024_param_file: "/etc/ssl/certs/dhparam_ffdhe2048.pem" # see https://www.postfix.org/postconf.5.html#smtpd_tls_dh1024_param_file
          message_size_limit: 52428800 # 50 MiB; max size of a incoming email
          mailbox_size_limit: 0 # 0 == unlimited; max mailbox size should be controlled by the mailserver
          relayhost: "[relay.example.org]:25"
          header_size_limit: 4096000
        run_postfix_mastercf_settings:
          smtps:
            type: "inet"
            private: "n"
            unpriv: "-"
            chroot: "n"
            wakeup: "-"
            maxproc: "-"
            command_args: "smtpd"
        run_postfix_relay_domains_manage: true
        run_postfix_relay_domains_list:
          - "example.com"
        run_postfix_transport_manage: true
        run_postfix_transport_map:
          "example.com": "lmtp:unix:/var/spool/postfix/private/lmtp-dovecot"
          "example.net": "lmtp:127.0.0.1:24"
          "example.org": "[192.168.0.1]"
          "foo.example.org": "[mailrelay.foo.example.org]:123"
        run_postfix_virtual_manage: true
        run_postfix_virtual_aliasdomains_list:
          - "example.com"
          - "example.net"
          - "example.org"
        run_postfix_virtual_alias_map:
          "admin@example.com":
            - "admin@actual-destination.example.com"
          "distribution@example.com":
            - "user1@example.com"
            - "user2@example.com"
            - "user3@example.com"
            - "user3@@example.com"
          "@example.com": # catch-all for example.com
            - "office@example.com"
          "@example.net": # catch-all for example.net, rewrite all to @example.com
            - "example.com"
```

Uninstall:

```yaml
---

- name: "Initialize the foundata.postfix.run role"
  hosts: localhost
  gather_facts: false
  tasks:

    - name: "Trigger invocation of the foundata.postfix.run role"
      ansible.builtin.include_role:
        name: "foundata.postfix.run"
      vars:
        run_postfix_state: "absent"
```



## Supported tags<a id="tags"></a>

It might be useful and faster to only call parts of the role by using tags:

- `run_postfix_setup`: Manage basic resources, such as packages or service users.
- `run_postfix_config`: Manage settings, such as adapting or creating configuration files.
- `run_postfix_service`: Manage services and daemons, such as running states and service boot configurations.

There are also tags usually not meant to be called directly but listed for the sake of completeness** and edge cases:

- `run_postfix_always`, `always`: Tasks needed by the role itself for internal role setup and the Ansible environment.



## Dependencies<a id="dependencies"></a>

See `dependencies` in [`meta/main.yml`](./meta/main.yml).



## Compatibility<a id="compatibility"></a>

See `min_ansible_version` in [`meta/main.yml`](./meta/main.yml) and `__run_postfix_supported_platforms` in [`vars/main.yml`](./vars/main.yml).



## External requirements<a id="requirements"></a>

There are no special requirements not covered by Ansible itself.
