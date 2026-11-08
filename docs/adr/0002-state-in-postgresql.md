# 0002. Terraform state in PostgreSQL on CloudNativePG

**Status:** Accepted

## Context
The state must be locked against concurrent runs, backed up, and kept off
laptops. The homelab already runs CloudNativePG for its databases.

## Decision
- Backend `pg`, on a dedicated CloudNativePG cluster: native locking
  (advisory locks), no extra service to operate.
- Reached from the laptop through `kubectl port-forward`: the database is
  never exposed outside the cluster.
- Credentials from the cluster (the CloudNativePG application Secret),
  passed to Terraform through the `PG_CONN_STR` environment variable, never
  as an argument or in a file.

## Consequences
- Terraform needs a running k3s cluster. It cannot repair the Proxmox
  guests that host k3s while they are down; the homelab power-outage runbook
  covers that case.
- No CloudNativePG cluster of the homelab has backups yet. Until scheduled
  backups exist, the state can be rebuilt from the guests with the same
  `import` blocks (ADR 0004), which is the reason they stay in the code.

## Rejected alternatives
- **Local state file:** no locking, lives on one laptop, plain text (Terraform
  does not encrypt state).
- **S3 on the NAS (MinIO or Garage):** independent from k3s, but one more
  service to run on a 2 GB ARM NAS, for a state that changes a few times a
  month.
- **HCP Terraform:** the state, which holds the real inventory, would leave
  the homelab.
