# Ansible Debug Output Role (`debug_output`)

This role generates detailed debug reports for your Ansible inventory, variables, and runtime context.  
It separates host-specific data from cluster-wide data to avoid duplication and provides a comprehensive snapshot of your environment.

## Overview

- **Per-host files** (`{{ debug_output_dir }}/hosts/<inventory_hostname>.txt`): contain only the variables, facts, environments, and metadata for each host.  
- **Cluster files** (`{{ debug_output_dir }}/cluster/`): contain information common across all hosts, including group mappings, inventory graphs, inventory lists, and optionally all `hostvars`.

This separation makes it easy to inspect individual hosts without repeating large data structures like the full `hostvars` dictionary.

## Requirements

- Ansible 2.9 or newer.
- Fact gathering must be enabled (`gather_facts: true`) to include facts and environment variables.

## Role Variables

| Variable | Default | Description |
|---------|---------|-------------|
| `debug_output_dir` | `./.debug_output` | Directory to write all debug output files. |
| `debug_inventory_file` | `null` | Inventory file to use; if null, the role auto-detects from `ansible_inventory_sources`. |
| `debug_write_cluster_hostvars` | `true` | Whether to write a file containing all hostvars for the entire cluster. |
| `debug_exclude_keys` | *(see defaults)* | List of keys to exclude when filtering per-host variables. Modify this if you need to include or exclude specific keys. |

## Example Playbook

```yaml
- name: Generate full debug output report
  hosts: all
  gather_facts: true
  roles:
    - debug_output
```

After running this playbook, look in the directory specified by `debug_output_dir` for the generated reports.

## Customization

- **Change output directory**: override `debug_output_dir` in your playbook or inventory.
- **Disable hostvars dump**: set `debug_write_cluster_hostvars` to `false` if you don't need the full hostvars file.
- **Adjust variable filtering**: modify `debug_exclude_keys` in `defaults/main.yml` to include or exclude specific keys when filtering per-host variables.

## Notes

- Running this role on large inventories may generate sizable files, particularly if `debug_write_cluster_hostvars` is `true`.
- Ensure you have appropriate disk space in your working directory.

---

Created and maintained by your DevOps automation team. Contributions welcome!