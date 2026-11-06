# Setup for owner — external-reference use

This file is a checklist **pattern**, not a record of completed setup for every target project. The active agent maintains actual project-specific pending actions in the target's chosen tracking document or hand-off, with owner permission.

## For each application repository

- [ ] Give the agent read access to the **target application repository**.
- [ ] Make `Simooo1010/claude-project-playbook` accessible as a **separate reference** (or provide the relevant content).
- [ ] Explicitly ask the agent to adopt the playbook; selecting two repositories by itself is not enough.
- [ ] Confirm the agent reports the playbook commit SHA and successfully read the mandatory files.
- [ ] Agree on the target's approval mode (default: Strict) and authorize the product spec before coding.
- [ ] Set any required secrets in supported environment-variable/secret-manager controls, **never in chat or Git**.
- [ ] Connect external systems and grant narrowly scoped permissions only when required.
- [ ] Test preview URLs on the specified real device, or explicitly confirm why a mandatory release gate remains pending.
- [ ] Authorize production release separately unless explicitly delegated in the target's written settings.

The agent must state which steps are genuinely complete. It cannot enable provider capabilities, permissions, monitoring or recurring work merely by promising to do so.
