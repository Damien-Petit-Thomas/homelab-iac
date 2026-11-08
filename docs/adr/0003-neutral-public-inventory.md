# 0003. Public code, private inventory

**Status:** Accepted

## Context
This repository is public, as a showcase. The homelab inventory (addresses,
host names, VLANs, endpoints) is not secret in the cryptographic sense, but
publishing it maps the network for anyone.

## Decision
- The code is public: resources, modules, imports, CI.
- The inventory comes from an untracked `*.tfvars` file; a
  `*.tfvars.example` documents its shape with documentation addresses.
- Credentials (Proxmox API token, state database) come from Vault and the
  environment, never from files in the working tree.
- A pre-commit hook, also run in CI, rejects private IPv4 addresses outside
  `*.tfvars.example`.

## Consequences
- Plans and state hold the real values: plans are never committed
  (`.gitignore`), pull requests show a plan summary only.
- Guest identifiers (VMID) and generic names stay in the code: they are
  needed to import resources and reveal nothing about the network.
