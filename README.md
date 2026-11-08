# homelab-iac

Terraform for a two-node Proxmox homelab. It takes over guests that were
created by hand, without recreating them, and makes their configuration
reviewable: the first goal is boot behaviour (`onboot`, start order), because
after a power cut nothing restarted on its own.

**Status:** bootstrapping. Repository foundations first; the Proxmox
provider, the state backend and the guest imports follow in separate pull
requests.

## Design

| Concern | Choice | Record |
|---|---|---|
| State | PostgreSQL backend (`pg`) on a dedicated CloudNativePG cluster | [ADR 0002](docs/adr/0002-state-in-postgresql.md) |
| Public repository | Code only; the real inventory (addresses, host names, endpoints) stays out | [ADR 0003](docs/adr/0003-neutral-public-inventory.md) |
| Existing guests | Imported, never recreated: `prevent_destroy`, and a plan at zero changes before any change | [ADR 0004](docs/adr/0004-import-never-recreate.md) |
| Branch protection | Pull requests only, gated by `ci-ok` and `zizmor` | [ADR 0001](docs/adr/0001-solo-maintainer-branch-protection.md) |

## Known limit

The state backend runs on the k3s cluster, which itself runs on the Proxmox
guests this repository manages. While k3s is down, Terraform cannot run: the
power-outage runbook of the homelab stays the recovery path, and this
repository only prevents the outage from needing it.

## Development

See [CONTRIBUTING.md](CONTRIBUTING.md).
