# 0004. Existing guests are imported, never recreated

**Status:** Accepted

## Context
The Proxmox guests were created by hand and hold state (k3s control plane
and worker, VPN router). A Terraform resource whose declared configuration
differs from the real guest on an immutable attribute is replaced: for a
container, that is a destroy and a create, so its data is lost.

## Decision
- Guests enter Terraform through `import` blocks, never by creation.
- Every guest resource carries `lifecycle { prevent_destroy = true }`.
- An import is complete when `terraform plan` shows **zero changes**. Only
  then does a separate commit change an attribute, and its plan must show
  in-place updates only.
- Attributes the provider cannot read back, or that would force a
  replacement for no real change, go to `ignore_changes`, each with a
  comment giving the reason.

## Consequences
- The first pull requests change nothing on the hosts: they only describe
  them.
- A plan that proposes a replacement is a bug in the description, never
  something to apply.
