# Security policy

## Reporting a vulnerability

Report vulnerabilities **privately** through GitHub:
<https://github.com/Damien-Petit-Thomas/homelab-iac/security/advisories/new>
(Security tab, *Report a vulnerability*). Do not open a public issue.
You will receive an initial response within 7 days.

Include the affected file, reproduction steps or plan output (with
addresses and secrets removed), the potential impact, and a suggested fix.

## Supported versions

Only `main` is supported: this repository describes one live homelab and
has no releases.

## Supply chain

- Third-party actions are pinned to commit SHAs; our own reusable workflows
  follow an immutable major tag (`.github/zizmor.yml`).
- Development and CI tools are pinned in `mise.toml`; `mise.lock` records
  their checksums, and their build provenance where upstream publishes it.
- Dependency updates wait 7 days after an upstream release (Dependabot
  cooldown, mise `install_before`).
- Provider versions are pinned and `.terraform.lock.hcl` records their
  checksums for every platform used.
- No secret and no real inventory in this repository: credentials come from
  Vault and the environment, addresses from untracked tfvars (ADR 0003).

## OpenSSF Scorecard: accepted exceptions

| Check | Rationale |
|---|---|
| Code-Review | Single maintainer. Every change goes through a pull request gated by required status checks (`ci-ok`, `zizmor`) and an automatic Copilot review. Human approval becomes mandatory when a second maintainer joins ([ADR 0004](docs/adr/0004-solo-maintainer-branch-protection.md)). |
| Branch-Protection | Pull request required, required status checks, no bypass, force pushes and deletions blocked. Required approvers, code-owner review and last-push approval need a second person. |
| Fuzzing | No fuzzable code: Terraform configuration, validated by `terraform validate`, tflint and a reviewed plan. |
