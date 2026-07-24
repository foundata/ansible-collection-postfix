# Ansible collection: `foundata.postfix`

This repository contains the `foundata.postfix` Ansible Collection.

It provides resources to manage and use the [Postfix](https://www.postfix.org/) mail transfer agent (MTA) that routes and delivers electronic mail (email) via [SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) and [LMTP](https://en.wikipedia.org/wiki/Local_Mail_Transfer_Protocol).


<div align="center" id="project-readme-header">
<br>
<br>

**⭐ Found this useful? Support open-source and star this project:**

[![GitHub repository](https://img.shields.io/github/stars/foundata/ansible-collection-postfix.svg)](https://github.com/foundata/ansible-collection-postfix)

<br>
</div>



## Table of contents<a id="toc"></a>

- [Included content](#content)
- [Dependencies](#dependencies)
- [Licensing, copyright](#licensing-copyright)
- [Author information](#author-information)



## Included content<a id="content"></a>

### Role: `foundata.postfix.run`

The primary role in this collection to configure and maintain Postfix, including core service, mail routing, domains, relays, TLS, and related settings. [Its `README.md`](./roles/run/README.md) covers configuration, usage examples, and more:

<!-- ANSIBLE DOCSMITH TOC-FULL run START -->
- [Ansible role: `foundata.postfix.run`](roles/run/README.md#ansible-role-foundatapostfixrun)
  - [Table of contents](roles/run/README.md#toc)
  - [Features](roles/run/README.md#features)
  - [Example playbooks, using this role](roles/run/README.md#examples)
  - [Supported tags](roles/run/README.md#tags)
  - [Role variables](roles/run/README.md#variables)
    - [`run_postfix_state`](roles/run/README.md#variable-run_postfix_state)
    - [`run_postfix_autoupgrade`](roles/run/README.md#variable-run_postfix_autoupgrade)
    - [`run_postfix_service_state`](roles/run/README.md#variable-run_postfix_service_state)
    - [`run_postfix_maincf_settings`](roles/run/README.md#variable-run_postfix_maincf_settings)
    - [`run_postfix_mastercf_settings`](roles/run/README.md#variable-run_postfix_mastercf_settings)
    - [`run_postfix_relay_domains_manage`](roles/run/README.md#variable-run_postfix_relay_domains_manage)
    - [`run_postfix_relay_domains_list_tabletype`](roles/run/README.md#variable-run_postfix_relay_domains_list_tabletype)
    - [`run_postfix_relay_domains_list`](roles/run/README.md#variable-run_postfix_relay_domains_list)
    - [`run_postfix_access_manage`](roles/run/README.md#variable-run_postfix_access_manage)
    - [`run_postfix_access_recipient_map_tabletype`](roles/run/README.md#variable-run_postfix_access_recipient_map_tabletype)
    - [`run_postfix_access_recipient_map`](roles/run/README.md#variable-run_postfix_access_recipient_map)
    - [`run_postfix_access_sender_map_tabletype`](roles/run/README.md#variable-run_postfix_access_sender_map_tabletype)
    - [`run_postfix_access_sender_map`](roles/run/README.md#variable-run_postfix_access_sender_map)
    - [`run_postfix_aliases_manage`](roles/run/README.md#variable-run_postfix_aliases_manage)
    - [`run_postfix_aliases_map_tabletype`](roles/run/README.md#variable-run_postfix_aliases_map_tabletype)
    - [`run_postfix_aliases_map`](roles/run/README.md#variable-run_postfix_aliases_map)
    - [`run_postfix_canonical_manage`](roles/run/README.md#variable-run_postfix_canonical_manage)
    - [`run_postfix_canonical_recipient_map_tabletype`](roles/run/README.md#variable-run_postfix_canonical_recipient_map_tabletype)
    - [`run_postfix_canonical_recipient_map`](roles/run/README.md#variable-run_postfix_canonical_recipient_map)
    - [`run_postfix_canonical_sender_map_tabletype`](roles/run/README.md#variable-run_postfix_canonical_sender_map_tabletype)
    - [`run_postfix_canonical_sender_map`](roles/run/README.md#variable-run_postfix_canonical_sender_map)
    - [`run_postfix_generic_manage`](roles/run/README.md#variable-run_postfix_generic_manage)
    - [`run_postfix_generic_map_tabletype`](roles/run/README.md#variable-run_postfix_generic_map_tabletype)
    - [`run_postfix_generic_map`](roles/run/README.md#variable-run_postfix_generic_map)
    - [`run_postfix_relocated_manage`](roles/run/README.md#variable-run_postfix_relocated_manage)
    - [`run_postfix_relocated_map_tabletype`](roles/run/README.md#variable-run_postfix_relocated_map_tabletype)
    - [`run_postfix_relocated_map`](roles/run/README.md#variable-run_postfix_relocated_map)
    - [`run_postfix_transport_manage`](roles/run/README.md#variable-run_postfix_transport_manage)
    - [`run_postfix_transport_map_tabletype`](roles/run/README.md#variable-run_postfix_transport_map_tabletype)
    - [`run_postfix_transport_map`](roles/run/README.md#variable-run_postfix_transport_map)
    - [`run_postfix_virtual_manage`](roles/run/README.md#variable-run_postfix_virtual_manage)
    - [`run_postfix_virtual_aliasdomains_list_tabletype`](roles/run/README.md#variable-run_postfix_virtual_aliasdomains_list_tabletype)
    - [`run_postfix_virtual_aliasdomains_list`](roles/run/README.md#variable-run_postfix_virtual_aliasdomains_list)
    - [`run_postfix_virtual_alias_map_tabletype`](roles/run/README.md#variable-run_postfix_virtual_alias_map_tabletype)
    - [`run_postfix_virtual_alias_map`](roles/run/README.md#variable-run_postfix_virtual_alias_map)
    - [`run_postfix_smtp_sasl_password_manage`](roles/run/README.md#variable-run_postfix_smtp_sasl_password_manage)
    - [`run_postfix_smtp_sasl_password_map_tabletype`](roles/run/README.md#variable-run_postfix_smtp_sasl_password_map_tabletype)
    - [`run_postfix_smtp_sasl_password_map`](roles/run/README.md#variable-run_postfix_smtp_sasl_password_map)
  - [Dependencies](roles/run/README.md#dependencies)
  - [Compatibility](roles/run/README.md#compatibility)
  - [External requirements](roles/run/README.md#requirements)
<!-- ANSIBLE DOCSMITH TOC-FULL run END -->



## Dependencies<a id="dependencies"></a>

See `dependencies` in [`galaxy.yml`](./galaxy.yml).



## Licensing, copyright<a id="licensing-copyright"></a>

<!--REUSE-IgnoreStart-->
Copyright (c) 2025, 2026 [foundata GmbH](https://foundata.com/) (https://foundata.com)

This project is licensed under the GNU General Public License v3.0 or later (SPDX-License-Identifier: `GPL-3.0-or-later`), see [`LICENSES/GPL-3.0-or-later.txt`](LICENSES/GPL-3.0-or-later.txt) for the full text.

The [`REUSE.toml`](REUSE.toml) file provides detailed licensing and copyright information in a human- and machine-readable format. This includes parts that may be subject to different licensing or usage terms, such as third-party components. The repository conforms to the [REUSE specification](https://reuse.software/spec/). You can use [`reuse spdx`](https://reuse.readthedocs.io/en/latest/readme.html#cli) to create a [SPDX software bill of materials (SBOM)](https://en.wikipedia.org/wiki/Software_Package_Data_Exchange).
<!--REUSE-IgnoreEnd-->

[![REUSE status](https://api.reuse.software/badge/github.com/foundata/ansible-collection-postfix)](https://api.reuse.software/info/github.com/foundata/ansible-collection-postfix)



## Author information<a id="author-information"></a>

This [project](https://foundata.com/en/projects/) was created and is maintained by [foundata](https://foundata.com/).

Initially based on an [Ansible skeleton](https://foundata.com/en/projects/ansible-skeletons/) developed by [foundata](https://foundata.com/).
