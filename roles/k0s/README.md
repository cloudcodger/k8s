k0s
===

Configure `k0s` on a set of hosts and cluster them into controller and worker nodes.

Requirements
------------

No special requirements.

Role Variables
--------------

- `k0s_bin_dir` - Directory for the `k0s` binary (default: `/usr/local/bin` ^1).
- `k0s_config_dir` - Directory holding all configuration files (default: `/etc/k0s` ^1).
- `k0s_config_file` - File name for config file in the config directory (default: `k0s.yaml` ^1).
- `k0s_control_group` - The Ansible group for the Controller nodes (default: `control`).
  Hosts not in the group will be configured as worker nodes.
- `k0s_data_dir` - Directory for `k0s` to store its data (default: `/var/lib/k0s` ^1).
- `k0s_download_base` - The base URL for downloading the `k0s` binary (default: `https://github.com/k0sproject/k0s/releases/download`).
- `k0s_genesis_group` - The Ansible group for the Genesis node (default: `genesis`).
  This group must contain one and only one of the hosts in the `k0s_control_group` or you
  will get unpredictable results. This host will be created as the initial controller node.
- `k0s_install` - A ARGV list for `k0s install (controller|worker)`.
- `k0s_install_controller_opts` - An ARGV list of command options for control nodes (see `defaults/main.yml`).
- `k0s_install_token_file` - The ARVG list for specifying a join token file (see `vars/main.yml`).
- `k0s_install_worker_opts` - An ARGV list of command options for worker nodes (see `defaults/main.yml`).
- `k0s_token_file` - File name for join tokens (default: `k0s.token`).
- `k0s_version` - The version of the `k0s` binary to download (default: looked up from the URL `https://docs.k0sproject.io/stable.txt`).
- `k0s_worker_profiles` - A list of worker profiles to add into the configuration (default: `[]`).

^1: These defaults are also the `k0s` default values.

Special Variable Notes
----------------------

### `k0s_worker_profiles`

This must be a list of dictionaries that define profiles for the worker nodes. 

Example Playbook
----------------

```yaml
---
- name: Example with generic all defauilts install of 'k0s'.
  become: true
  hosts: k0s_ans

  roles:
    - role: cloudcodger.k8s.cloud_init_reboot
    - role: cloudcodger.k8s.k0s
```

```yaml
---
- name: Example install of 'k0s' with workers having maxPods of 220.
  become: true
  hosts: k0s_ans

  roles:
    - role: cloudcodger.k8s.cloud_init_reboot
    - role: cloudcodger.k8s.k0s
      k0s_worker_profiles:
        - name: default
          values:
            maxPods: 220

```
