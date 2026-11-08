# Contributing

## Toolchain

Every tool is pinned in `mise.toml` and locked in `mise.lock`; CI installs
the same versions with `jdx/mise-action`.

```bash
mise trust && mise install   # installs the pinned versions, checksums verified
pre-commit install           # git hooks, run from a mise-activated shell
mise run lint                # every hook on the whole tree, as in CI
```

## Changing infrastructure

1. Change the code on a branch; `terraform plan` must show only the intended
   changes. An existing guest is never replaced or destroyed
   ([ADR 0004](docs/adr/0004-import-never-recreate.md)).
2. Open a pull request with the plan summary (resource counts and the
   changed attributes, never addresses or secrets).
3. After the merge, apply from `main`.

The real inventory lives in an untracked `*.tfvars` file
([ADR 0003](docs/adr/0003-neutral-public-inventory.md)); a pre-commit hook
rejects private IPv4 addresses outside `*.tfvars.example`.

## Commits

[Conventional Commits](https://www.conventionalcommits.org/):
`feat(proxmox): start the router container first`.

## Pull requests

`main` accepts pull requests only. A pull request merges when `ci-ok` and
`zizmor` pass, by rebase only. A new CI job must be added to the `needs` of
`ci-ok`, never to the ruleset
([ADR 0001](docs/adr/0001-solo-maintainer-branch-protection.md)).

## Repository settings

The branch ruleset is versioned in `.github/rulesets/`. After changing it in
GitHub, export it with `scripts/export-ruleset.sh` and commit the result.
