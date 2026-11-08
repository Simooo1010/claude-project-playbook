# Universal Project Playbook

A provider-neutral, **externally referenced** development workflow for Claude Code, Codex and other AI coding agents: spec first; small, verified steps; human approvals; independent review; disciplined model/subagent use; honest hand-offs.

**This is a central reference repository, not a GitHub template you must copy.** Keep the *application repository* separate. In your agent's session select/connect the application repo and make this playbook accessible as another repo or GitHub source, then explicitly tell the agent to adopt this playbook.

## Quick start — no copying

1. Open an agent session with access to the **target application repository**.
2. Make this reference repository accessible: [Simooo1010/claude-project-playbook](https://github.com/Simooo1010/claude-project-playbook). If it cannot be accessed in that session, share the required files or enable access first.
3. Send this activation prompt:

> Work in **[TARGET_OWNER/TARGET_REPO]**. Adopt **https://github.com/Simooo1010/claude-project-playbook** as the governing development playbook for this project. Do not copy the playbook into the target repository. First read `AGENTS.md`, `docs/WORKFLOW.md`, `docs/HOW-TO-WORK-WITH-AGENTS.md`, and `docs/PROVIDER-ADAPTERS.md` from the **playbook repo** in full. Record the exact playbook commit you read, then inspect the target repo's instructions and current state. Perform WORKFLOW §0 preflight and tell me what you verified, the active approval mode, any blockers, and what you need from me. Do not write code before the spec approval gate. Follow every applicable rule in subsequent steps, including §9's model/effort policy and Astra restrictions.

4. Approve the spec when it accurately describes the work. Continue step by step using the approval mode selected for that target project.

### Fast mode — for projects that must be built quickly

For prototypes, MVPs and internal tools, add this line to the activation prompt:

> Use **Fast profile** (WORKFLOW §13): write a one-page brief instead of a full spec, ask at most one round of blocking questions, build autonomously in vertical slices on one branch/PR, and give me one short hand-off at the end. Stop only for the §13.2 hard stops.

Fast mode drops the per-step approvals, the full spec ceremony, per-step hand-offs and the mandatory pre-release QA gates. It keeps secret safety, scope limits, model policy and honest reporting. Do not use it for payments, auth, personal data or a first release to real users, or switch to Standard (§13.4) before those.

**Important:** Merely selecting both repositories, attaching a file, or mentioning this repository does **not** automatically cause an agent working in another repository to read and obey it. The explicit instruction and preflight above are essential. A chat can apply instructions within its accessible context but cannot guarantee future sessions or unsupported actions.

## Documents

- [AGENTS.md](AGENTS.md) — short entrypoint for this reference repository
- [CLAUDE.md](CLAUDE.md) — Claude compatibility import
- [docs/WORKFLOW.md](docs/WORKFLOW.md) — **canonical method**, including §9 subagents/models
- [docs/HOW-TO-WORK-WITH-AGENTS.md](docs/HOW-TO-WORK-WITH-AGENTS.md) — exact prompts for external repo, project/workspace, chat and resumed sessions
- [docs/PROVIDER-ADAPTERS.md](docs/PROVIDER-ADAPTERS.md) — Claude/Codex specifics and optional local configuration
- [docs/SPEC.template.md](docs/SPEC.template.md) — shape of the **target project's** spec
- [SETUP-FOR-OWNER.md](SETUP-FOR-OWNER.md) — checklist pattern for human-only actions

## Source of truth and versions

The target repo owns its application code and product spec. This repo owns the general working method. Record the **playbook commit SHA** in a project note, spec decision log or conversation hand-off, and keep it fixed for the current step. New updates to the playbook are not silently adopted halfway through work: review them before changing the pinned reference.

This repo contains instructions and documentation, not a background monitoring service or an automatic cross-repository enforcement mechanism.
