# Ansible Collection - cloudcodger.k8s

A collection of roles for configuring a Kubernetes cluster.

# Roles

## `cloudcodger.k8s.cloud_init_reboot`

This role will wait for a system created with a Cloud-Init script to finish the initial `apt upgrade` it performs on the first boot.
It then checks if `/var/run/reboot-required` exist and performs a reboot if it does and this is the first time it has run on the system.

## `cloudcodger.k8s.k0s`

This role will configure the hosts as a Kubernetes clusting using `k0s`.
