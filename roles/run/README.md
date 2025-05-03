# Ansible role: `foundata.postfix.run`

The `foundata.postfix.run` Ansible role (part if the `foundata.postfix` Ansible collection).

It provides automated configuration management of Postfix across major platforms. It does not explain how to create a good Postfix configuration that suits your requirements; that task remains for you to accomplish.



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
