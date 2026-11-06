# Universal Project Playbook — agent entrypoint

This repository is the **central reference playbook**, not the application repository. The owner may ask an AI agent to work on another repository and explicitly adopt this repository as its governing development workflow. The owner does **not** need to copy these files or use GitHub's template feature.

## Activate only when explicitly adopted
When the owner designates this playbook, read `docs/WORKFLOW.md` completely and use `docs/HOW-TO-WORK-WITH-AGENTS.md` for external-repository onboarding. The authoritative method is `docs/WORKFLOW.md`; this entrypoint is only a pointer.

Before doing project work, identify the *target* repository separately from this *playbook* repository, record the playbook commit/ref actually read, inspect the target's applicable instructions and spec, and perform the startup preflight in WORKFLOW §0. Repeat the step preflight for every implementation step. Do not assert that reading this file in the playbook repository automatically installs instructions in other repositories.

If you cannot read a required file or cannot satisfy a mandatory gate, disclose that precisely and stop the affected operation rather than pretending compliance. Provider limitations, owner approvals, security restrictions and actual permissions still apply.

Never put secrets in the repository or chat. Never infer that access to a GitHub repository authorizes writes to other repositories.

## For changes to this playbook itself
Preserve the provider-neutral workflow and its original spec-first, approval, step-loop and QA principles. Keep the model policy in WORKFLOW §9 as the single canonical rule; provider-specific examples belong in `docs/PROVIDER-ADAPTERS.md`. Changes to governance should be reviewed on a branch before merge.
