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
- `k0s_controller_logging` - Add `--logging` to Controller nodes (deault: `''`).
- `k0s_data_dir` - Directory for `k0s` to store its data (default: `/var/lib/k0s` ^1).
- `k0s_disable_components` - A list of components to disable on the controller nodes (default: `['metrics-server']`).
- `k0s_download_base` - The base URL for downloading the `k0s` binary (default: `https://github.com/k0sproject/k0s/releases/download`).
- `k0s_enable_cloud_provider` - Add `--enable-cloud-provider` on all nodes ^2 (default: `false`).
- `k0s_enable_dynamic_config` - Add `--enable-dynamic-config` on controller nodes (default: `false`).
- `k0s_enable_k0s_cloud_provider` - Add `--enable-k0s-cloud-provider` on controller nodes (default: `false`).
- `k0s_enable_metrics_scraper` - Add `--enable-metrics-scraper` on controller nodes (default: `false`).
- `k0s_enable_worker` - Add `--enable-worker` on controller nodes (default: `false`).
- `k0s_genesis_group` - The Ansible group for the Genesis node (default: `genesis`).
  This group must contain one and only one of the hosts in the `k0s_control_group` or you
  will get unpredictable results. This host will be created as the initial controller node.
- `k0s_iptables_mode` - Set `--iptables-mode` (default: `auto` ^1).
  Valid values are: `nft`, `legacy`, or `auto`.
- `k0s_kube_controller_manager_extra_args` - Options to set on controller nodes (default: `''`).
- `k0s_kubelet_extra_args` - Set `--kubelet-extra-args` on all worker nodes ^2 (default: `''`).
- `k0s_no_taints` - Add `--no-taints` on controller nodes (default: `false`).
- `k0s_profile_name` - Add `--profile` to worker nodes ^2 (default: `default` ^1).
- `k0s_single` - Add `--single` on controller nodes (default: `false`).
- `k0s_taints` - List of node tains (default: `''`).
- `k0s_token_file` - File name for join tokens (default: `k0s.token`).
- `k0s_version` - The version of the `k0s` binary to download (default: looked up from the URL `https://docs.k0sproject.io/stable.txt`).
- `k0s_worker_logging` - Add `--logging` to Worker nodes (default: `''`).
  This is not added to controller nodes with `--enable-worker` or `--single`.
- `k0s_worker_profiles` - A list of worker profiles to add into the configuration (default: `[]`).

^1: These defaults are also the `k0s` default values.
^2: These only apply to controller nodes that include `--enable-worker` or `--single`.

Special Variable Notes
----------------------

### `k0s_profile_name`

The default for this in `k0s` is the `default` profile. See `k0s_worker_profiles`.

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
- name: Example install of 'k0s' with workers having maxPods of 220 and changing controller logging.
  become: true
  hosts: k0s_ans

  roles:
    - role: cloudcodger.k8s.cloud_init_reboot
    - role: cloudcodger.k8s.k0s
      k0s_controller_logging: "etcd=warn,containerd=warn,konnectivity-server=0"
      k0s_worker_profiles:
        - name: default
          values:
            maxPods: 220

```
