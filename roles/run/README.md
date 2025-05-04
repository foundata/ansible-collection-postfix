# Ansible role: `foundata.postfix.run`

The `foundata.postfix.run` Ansible role (part if the `foundata.postfix` Ansible collection).

It provides automated configuration management of Postfix across major platforms. It does not explain how to create a good Postfix configuration that suits your requirements; that task remains for you to accomplish. However, the [examples playbooks](#examples) may help you a bit with that.



## Table of contents<a id="toc"></a>

- [Role variables](#variables)
- [Example playbooks, using this role](#examples)
- [Supported tags](#tags)
- [Dependencies](#dependencies)
- [Compatibility](#compatibility)
- [External requirements](#requirements)



## Role variables<a id="variables"></a>

See [`defaults/main.yml`](./defaults/main.yml) for all available role parameters and their description. [`vars/main.yml`](./vars/main.yml) contains internal variables you should not override (but their description might be interesting).

Additionally, there are variables read from other roles and/or the global scope (for example, host or group vars) as follows:

- None right now.



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
        run_postfix_handle_relay_domains: true
        run_postfix_relay_domains:
          - "example.com"
        run_postfix_handle_transport_maps: true
        run_postfix_transport_maps:
          "example.com": "lmtp:unix:/var/spool/postfix/private/lmtp-dovecot"
          "example.net": "lmtp:127.0.0.1:24"
          "example.org": "[192.168.0.1]"
          "foo.example.org": "[mailrelay.foo.example.org]:123"
        run_postfix_handle_virtual: true
        run_postfix_virtual_alias_domains:
          - "example.com"
          - "example.net"
          - "example.org"
        run_postfix_virtual_alias_maps:
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
