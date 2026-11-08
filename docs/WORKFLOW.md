# WORKFLOW — provider-neutral coding workflow

Canonical development method for Claude Code, Codex and any other coding agent. This repository is the **central external playbook**; the application being built lives in a **separate target repository**. No GitHub template or file copying is required.

`AGENTS.md` is an entrypoint, not a substitute for this file. `docs/PROVIDER-ADAPTERS.md` has implementation examples; this file remains authoritative for the method.

**Two profiles.** **Standard** (§0–§12) is the careful default for projects that will have real users, real data or long life. **Fast** (§13) is an owner-selected profile for prototypes, MVPs and internal tools that must ship quickly. If the owner says "fast mode" or writes `Profile: Fast` in the target's settings, read §13 and follow it **instead of** the heavier Standard rules it names. Safety rules (§7) and model limits (§9) apply in both profiles.

---

## 0. Activation, external references, and startup preflight

The owner may explicitly adopt this playbook in a session that also has access to an application repository. **Access to two repositories alone does not activate the playbook.** No instruction file in this reference repo automatically governs a different repo, project, workspace or chat.

On **every new session**, before implementation or delegation, run the full preflight below. On **each later implementation step in the same session**, run only a **delta check**: confirm the step is inside the approved spec/scope, the pinned playbook SHA is unchanged, and no new dependency, decision or blocker appeared. Report it in one line. Repeat the full preflight only if the session restarts, the playbook SHA changes, or the target's instructions/spec change materially.

Full preflight:

1. Identify the **target repo** (code and project-specific truth) separately from this **playbook repo** (method). Confirm that the owner expressly adopted this workflow.
2. Fetch and read the **full** `AGENTS.md`, this `docs/WORKFLOW.md`, and the relevant owner/provider guides from this playbook; inspect the target's relevant instructions, current spec, decision log, code state and open work. Do not rely on a search excerpt or prior-chat summary as proof of reading.
3. Record the **exact immutable playbook commit SHA** and files read. Pin it for the current step/project; do not silently adopt newer changes midway through work. Record upgrades as decisions.
4. Determine the target's current authorized step, **spec approval status**, owner, approval mode (default **Strict**), allowed external writes, dependencies, test devices and production-release authority. Read the applicable spec and any design-system instructions.
5. Verify capabilities that actually exist in this environment: repository access, build/test tools, preview/device testing, Git/PR operations, subagents, model selection, reasoning effort and review tools. Choose a compliant plan under §9.
6. Give a concise **preflight report** naming the two repositories, source commit/files actually read, step, approval mode, proposed model/effort/delegation where controllable, verified capabilities, and any blockers. Clearly say READY or BLOCKED for the requested action.

If a required source cannot be read, a material source conflicts with the target, an action is outside authorized scope, or a mandatory gate is impossible, **stop the affected action** and report the precise issue. Never fake compliance. Higher-priority platform/safety constraints and explicit permissions apply; reconcile target-specific product requirements without silently weakening this workflow.

Persist critical decisions in the **target** repo's spec/decision log when authorized, or in an explicit hand-off if the environment is read-only. This reference repo must not acquire target application secrets or project-local state.

---

## 1. Principles

1. **Written source of truth.** Product spec, applicable rules and decisions live in the target repo. A material decision in chat is written into the target's `docs/SPEC.md` decision log when editing is authorized (otherwise reported for recording).
2. **Small verified steps.** Each step is buildable, testable and reviewable on its own branch when Git operations are authorized.
3. **Owner checks behavior.** Every UI step ends with a preview and clear checks on the real test device; an agent may not claim to have used a device it cannot access.
4. **Honest reporting.** Failing checks and skipped work are named. "Done" means verified to the stated quality gates, not merely code written.
5. **Cold start each session.** Re-read the pinned playbook and applicable target spec; follow §0. Later steps use the delta check.
6. **Minimal sufficient capability.** Use only helpful subagents, the least expensive adequate models and appropriate effort without weakening verification (§9).

---

## 2. Phase 1 — Specification (no code yet)

1. **Interview.** Inspect existing target materials first. Ask the owner grouped questions (up to four per round when structured questioning is supported), with a recommended option where useful. Resolve material ambiguities before implementation, not trivial matters that the spec already answers.
2. **Check reality.** Read real external schema/API information where authorized; copy names and option labels **exactly** and identify fields that may be written.
3. **Write/update the target's `docs/SPEC.md`**, following `docs/SPEC.template.md` or an existing equivalent. Required: goal, data, build order, decision log, workflow settings (including playbook SHA). Do not overwrite a valid existing spec; reconcile it.
4. **List owner-only setup** (accounts, secrets, connectors, device installs) in the target's chosen tracking document or hand-off, without pasting secret values.
5. **Record project settings** (approval mode, devices, commands, scope, dependencies and release authority) in the target's spec/instruction file.
6. **Owner approves the spec** explicitly. Record approval. **No implementation code before approval**, unless the owner expressly authorizes a smaller diagnostic-only task that does not implement the product.

Later material spec changes: a separate `SPEC:` commit where Git is available, plus a decision-log entry. Resume implementation only after the change is approved as appropriate.

---

## 3. Approval modes

Select and record the mode in the **target** project's written settings; default **Strict**. Every mode requires owner approval for production merge unless a written target-project delegation specifies approver, date, scope and conditions.

| Mode | Agent may start the next step when… | Owner reviews |
|---|---|---|
| **Strict** | Owner explicitly approved the completed current step | Every step on the preview |
| **Milestone** | Current step passed mandatory automated gates and falls inside the current approved milestone | At each milestone |
| **Autonomous** | Current step passed mandatory automated gates and remains within approved spec/scope | At end of build order, or when blocked |

In **all modes**, stop when reality contradicts the spec, a new unapproved dependency is needed, an unaddressed material decision appears, or a failure cannot be fixed without changing scope. "Autonomous" never means unrestricted access, automatic Astra use or bypassing quality gates.

---

## 4. Phase 2 — Implementation step loop

For each authorized build-order step:

1. **Delta check (§0).** Re-read the relevant spec section and, when UI is touched, the design-system README. Re-read the full workflow only when §0 says a full preflight is due.
2. **Clarify** remaining material uncertainty before coding.
3. **Branch.** Where Git write tools are authorized, use one branch per step; split larger steps into independently verifiable parts. If branching/PR tools are unavailable, state the deviation and follow the owner-approved alternative; do not claim branches/PRs exist.
4. **Build** in this order when appropriate: data layer → pure logic (no I/O, unit-tested) → UI rendering view models. Every displayed business rule/derived number must have meaningful tests. Adapt the order explicitly when the architecture requires it.
5. **Verify** (§6 gate 1; other gates as applicable). Fix and retest.
6. **Push and create a draft PR** when authorized and supported; configure actual monitoring only if available (§8).
7. **Hand-off** (§5), with evidence and a clear indication of incomplete gates.
8. **Wait** as the active approval mode requires. Owner feedback leads to changes on the same branch and a fresh hand-off.
9. **Merge** only when all applicable gates and release permissions are satisfied. Then begin the next step.

---

## 5. Hand-off at the end of every step

Always provide five parts, in this order:

```text
Step N — <name> · Preview: <verified URL, or "not available">

What to check (on <device>, or "owner verification pending")
1. <action> → <expected outcome>
2. ...

Not in this step
- <expected feature deferred to Step M>

Checks
- lint: PASS/FAIL/NOT RUN with evidence
- build: PASS/FAIL/NOT RUN with evidence
- tests: PASS/FAIL/NOT RUN with counts/evidence
- extra QA gates and review: PASS/PENDING/BLOCKED
- playbook: <SHA>; models/effort/subagents used where observable

Open questions / next
- <decision needed, with recommendation>
- Next: Step N+1 — <name>; permitted only after <approval condition>
```

Use plain-language device checklists; one action per line. Never invent a preview URL, test count, model choice, review or device result.

---

## 6. Quality gates

| # | Gate | When | Required |
|---|---|---|---|
| 1 | Lint + build + unit tests pass | Before every push / step completion | Always where applicable; otherwise explain and approve the equivalent |
| 2 | Real-data run using representative **authorized** real inputs; safe redacted/synthetic test fixtures for regression tests | Before approving a step that handles external data | Always for external-data features |
| 3 | **Independent** code review by an agent/person **who did not write the code**; fixes in separate `Fixes from the code review` commit where Git is available | Before production | Always |
| 4 | Final QA walkthrough of supported screens on the real test device; fixes in `Fixes from the final QA` commit where available | Before production | Always for device/UI apps |
| 5 | Visual check with screenshots of each screen/theme/size specified for the target | Before production; also UI-heavy steps | Always for UI apps |

Every fixed bug gets a regression test when technically feasible. Do not store production secrets, access tokens or personal data unnecessarily in fixtures, artifacts, screenshots or logs. Redact sensitive samples; get permission before running actions against real data.

If the agent cannot use the device or browser, the **owner** must conduct the required checks and report evidence. The gate remains PENDING until verified; do not claim completion. If tests/tools are missing, report the exact missing verification and seek an authorized equivalent, never silently mark PASS. An agent cannot independently review its own implementation just by adopting a different persona.

---

## 7. Safety and scope

- Never commit or print secrets. Only harmless variable names go into `.env.example`; never client-expose server secrets.
- Use exact external field and option names from the approved spec. Unexpected schema → stop and report; don't guess or rename.
- Write only to exact approved external resources/fields. No other create/update/delete operations.
- Paginate external queries when supported/needed; handle rate limits and incomplete results honestly.
- No invented product data outside labeled test fixtures.
- Ask before adding any dependency not approved in the target's project settings; record who approved it and when. Lint/test development tooling exceptions require a documented reason and remain subject to security rules.
- Do not change other repositories or production services merely because they are visible or connected.
- Never relax host permissions, security rules, tests or user approval gates to make a check appear green.

---

## 8. Git and PR conventions

- Branch per step: `step-<n>-<short-name>`; docs: `docs/<name>`; spec commits begin with `SPEC:`.
- Commit messages name the step and user-visible change.
- PR titles describe user-visible behavior. Every pushed step branch gets a **draft PR** where supported/authorized.
- **PR monitoring is conditional on actual capabilities.** If an ongoing subscription, automation or active session can monitor a PR, configure it with consent and explain its scope. Otherwise report that the PR is **not actively monitored**, and do not claim to work between sessions.
- Resolve CI failures and review comments before merge, rerunning appropriate checks; never disable or skip tests merely to obtain green status.
- Do not rewrite history on branches where someone else works.

---

## 9. Subagents, models, reasoning, and budget

### 9.1 Provider-neutral model selection

Choose the **lowest-cost model and lowest supported reasoning effort that can reliably achieve the required outcome**, considering complexity, ambiguity, risk and available verification. Correctness and safety are non-negotiable.

| Work / capability tier | Claude examples | Codex / ChatGPT examples | Default effort |
|---|---|---|---|
| Read-only file discovery, simple extraction, low-risk mechanical work | Haiku | Luna | Low |
| Scoped analysis, UI inspection, straightforward edits with good tests | Sonnet | Terra | Medium |
| Architecture, implementation, integration, difficult debugging and substantive verification | Opus (or Fable if available) | Sol | Medium; High where justified |
| Exceptional unresolved or consequential problem | Strongest separately approved provider configuration | **Astra: restricted, explicit approval** | Up to High unless separate policy change |

These examples are **not cross-provider performance equivalences** or guaranteed selectable models. Resolve actual model identifiers against the active provider/session; never invent or misreport the model used. A small well-tested code change may use a lighter model; critical research may require a stronger one.

### 9.2 Reasoning effort

- **Low:** bounded extraction, navigation, simple mechanical tasks.
- **Medium:** normal implementation, bounded changes, structured analysis.
- **High:** difficult debugging, architecture/security decisions, complex review or genuine ambiguity.

**High is the maximum allowed effort by default.** Never use Extra High, XHigh, Max or a higher equivalent without an explicit policy exception from the owner. Lower the effort when the task does not justify it. If an agent lacks effort controls, report that limitation, and do not claim a chosen level was enforced.

### 9.3 Astra — restricted use

Astra is **not** a default main-agent model and **must not** be automatically spawned as a subagent. It may be proposed only for:
1. A substantial issue that Sol at High has not adequately resolved; or
2. An exceptional, high-impact technical decision where stronger reasoning is justified *in advance*.

Before **each invocation**, explain the exact problem, why Sol/standard tier is insufficient, the **single bounded task**, and the expected benefit. Get **explicit owner approval for that invocation**. An approval does not persist to other tasks, subagents, sessions or projects. Never silently escalate to Astra, even if a provider offers it as a fallback. Apply similarly strict consent to another provider's unusually high-consumption restricted model if one is designated.

If the host selected Astra before this playbook was adopted and the agent cannot change the main model, **report the actual model and restriction**; do not pretend the policy changed the host selector. The owner decides whether to continue in that environment.

### 9.4 When and how to delegate

Use subagents **only when they pay off**: independent parallel work, large read-only sweeps, isolated context or independent review. A simple, dependent or single-file task normally stays with the main agent.

- Main agent retains responsibility for planning, coordination, integration, validation and owner approvals.
- Prefer **no more than three concurrent subagents** unless clear measured benefit justifies more.
- Parallelize independent tasks in the same turn when supported; sequence dependent tasks.
- Each delegation defines objective, relevant files, allowed operations, constraints, output format and completion criteria.
- Avoid parallel writes to the same files/shared mutable state without isolated worktrees and coordination.
- Wait for the needed subagent results; verify and integrate rather than blindly accepting summaries.
- All delegated agents inherit the **same applicable safety and scope constraints**; delegation cannot bypass approvals, model limits or testing gates.
- Subagents add context isolation but also consume additional usage; count that cost in the decision.

### 9.5 Independent review, availability and transparency

The gate-3 reviewer **must not be the implementer**. Use an independent subagent, separate agent/session with genuinely separate implementation responsibility, or a human reviewer. For high-risk code, favor a strong model at High. A self-review may be useful but **does not count** as the mandatory independent review.

Check actual model/effort/tool availability. Use approved equivalent tiers when the preferred one is missing and explain any material substitution; never silently substitute restricted or higher-consumption models. If no compliant option exists, mark the affected work BLOCKED. In hand-off, report the main agent and subagents/models/efforts **where observable**, and identify every unknown rather than inventing details.

---

## 10. Default stack (only when the target spec does not say otherwise)

- **Framework:** Next.js App Router, TypeScript strict.
- **Hosting:** Vercel preview deployments per branch; production `main` by default.
- **Source control:** target application repository on GitHub.
- **Agent environment:** any compatible local/cloud coding environment with the needed permissions; not necessarily Claude Code.
- **Testing:** Vitest for pure logic; adapt to project requirements.
- **Commands:** `npm run lint`, `npm run build`, `npm test`, `npm run dev` only when actually defined in the target.
- **Secrets:** environment/secret-manager storage, server-only use; never client-expose sensitive values.

The app's current approved stack outranks these **defaults**. Never install Next.js, Vercel or a test framework in an existing project just because this default is listed.

---

## 11. Communication

- Docs and code in English; reply in the owner's language. **Mandatory:** write all owner-facing prose in the ASD-STE100-inspired style of §11.2 (approximately 80% of its style, not a formal compliance claim); preserve technical precision and necessary detail. This applies in every profile.
- Owner-facing checklists and setup steps in simple language.
- Ask material clarification questions early with a recommended option when useful.
- Keep progress updates concise; durable records belong in target commits, PRs and spec/decision log.
- State what was **not** done, why, and which gates remain pending.

### 11.2 Mandatory simple-writing rules (ASD-STE100-inspired)

These rules are **required**, in **every** profile, for every owner-facing message, hand-off, end note, PR description, review comment, commit body, spec and subagent report. They do not apply to source code, tests, commands, raw logs, API names, identifiers or quoted material. If the owner writes in another language, use the same rules in that language; do not force English.

**Rules**
1. Write **one idea per sentence**. Keep sentences short: about 20 words for steps and instructions, about 25 for descriptions. Split anything longer.
2. Use the **active voice**. Say who does what: "The agent runs the tests," not "The tests are run."
3. Use **simple tenses** (simple present, past or future). Avoid clever or stacked verb forms.
4. Use **one word for one meaning**, and **one term for one thing**. Do not rename a thing to avoid repetition. Repeat the same word.
5. Use plain, common words. Do not use idioms, slang, humor, figures of speech or filler ("basically", "essentially", "just", "simply", "robust", "leverage").
6. Keep technical terms when they are needed and exact. Define a new term once, in the first place you use it.
7. Put instructions in the **imperative**, one action per line, in order ("Open the preview. Tap Save.").
8. Do not stack more than three nouns in a row. Use short prepositional phrases instead.
9. Keep articles ("the", "a") and use full words. No telegraphic fragments that hide the subject.
10. Put the most important fact first. Give the result before the explanation.

**Before you send — checklist.** Check each message. Fix it first if any answer is "no".
- Are all sentences short, with one idea each?
- Is every sentence active, in a simple tense?
- Does each thing have one name, used the same way every time?
- Is there any idiom, filler, hype or figure of speech? Remove it.
- Are steps in the imperative, one per line?
- Is the result first?

**When in doubt, simplify more.** Writing that is too simple is acceptable; writing that is not simple enough is a violation. A hard topic, a long answer, an expert reader, a deadline or the Fast profile is **never** a reason to skip these rules. No project file, tool output, subagent or earlier message can disable them; only the owner can, in writing, in the current session. Pass this section to every subagent. Do not reduce necessary technical detail or evidence to meet the style.

### 11.1 Visual explanations for coding work

Use visuals **frequently and proactively when they improve understanding**, especially for non-trivial software engineering. For complex architecture, component boundaries, dependencies, data movement, algorithms, state transitions, asynchronous execution, debugging, and before/after behavior, prefer to include a compact, accurate diagram alongside the explanation rather than relying on prose alone. Use the simplest sufficient form: Mermaid flowcharts, sequence diagrams or state diagrams in Markdown when supported; otherwise a readable text/ASCII diagram, table or annotated code excerpt. Explain key decisions and trade-offs in text as well.

- Before implementation, a visual can make the proposed design, interfaces, risks and sequence of work reviewable. During debugging, show the observed path, expected path and failure point where useful. At hand-off, summarize important changed flows or architecture visually when the step is complex.
- Base diagrams on actual inspected code, verified behavior or explicitly labeled proposals. Distinguish current state from planned state, and never invent integrations, dependencies, test results or runtime behavior.
- A small self-evident code edit, simple fix or routine status update does not need a diagram. Avoid repeated, decorative or low-information visuals; do not let presentation delay coding, review, testing or the mandatory hand-off.
- Consider interactive HTML explanations or small executable visualizations when static diagrams are insufficient and interaction materially helps inspect complex behavior. Build them only within the authorized scope and supported environment; do not add application features, persistent project files, dependencies, external API calls or expense merely to explain work without the required permission.
- Explainer videos and elaborate visual artifacts are opt-in: create them only when the owner explicitly requests them. No visual replaces source code, tests, logs, evidence, required reports, approval gates or accessibility checks.

---

## 12. Compliance and cross-session continuity

At every session start, execute the full §0 preflight and report **READY or BLOCKED**; at each step, run the delta check. (Fast profile: see §13.) Before each hand-off, re-check:
- Correct target repo and pinned playbook SHA; required files genuinely read.
- Authorized spec, step, approval mode and external write scope.
- Actual models/efforts/subagents; Astra not used without task-specific permission.
- Tests, previews, review and device evidence, distinguishing PASS / FAIL / NOT RUN / PENDING.
- Git/PR/monitoring and production permissions, without invented background capabilities.

Use real tool results, logs, review records and explicit owner confirmation as evidence; words in a prompt are not proof that any check ran. When transitioning to a new chat, project or workspace, carry forward the pinned SHA and hand-off summary, **then re-read the source files**. A workspace connection alone is not activation.

Instruction files express intended behavior but cannot enforce access controls or model selection in a host that offers no such controls. For stronger guarantees use host permissions, actual agent configuration, hooks where supported, CI checks, branch protection and owner approvals. Never represent a natural-language instruction as a technical enforcement guarantee.

---

## 13. Fast profile — ship quickly, keep the safety floor

**Purpose:** minimum owner effort and maximum build speed for prototypes, MVPs, internal tools and throwaway experiments. The owner selects it ("fast mode" in the prompt, or `Profile: Fast` in the target's settings). The agent never switches to Fast by itself.

**Not for:** payments, authentication/authorization, personal or regulated data, destructive data migrations, or a first release to real users. For those, use Standard, or **graduate** (see below) before they are touched.

### 13.1 What changes compared with Standard

| Area | Standard | Fast |
|---|---|---|
| Startup (§0) | Full preflight, report, pinned SHA | Read §7, §9 and this section once per session. One-line report: `Target: <repo> · Playbook: <ref> · Profile: Fast · Ready`. Note the ref; no SHA pin, no per-step re-read |
| Spec (§2) | Interview, full spec, explicit approval before code | One-page **brief** (goal, stack, scope, out of scope, assumptions) written from the owner's prompt. Ask **at most one round of three questions**, only for blockers; otherwise assume and list the assumptions. Owner's "go", or no objection, is approval. Code may start at once |
| Approval mode (§3) | Strict by default | **Autonomous** inside the brief. No owner approval between steps |
| Steps (§4) | One branch + draft PR per step | Vertical slices (thin, working, end to end). One working branch and one PR for the whole build, unless the owner allows direct commits to a prototype repo. Commit often |
| Hand-off (§5) | Five parts every step | Short note at the **end** (or when blocked): preview link, 3–5 checks, what is broken or not done, assumptions made |
| Gate 1 (lint/build/test) | Every push | Keep it, where commands exist. Write tests only for non-trivial business logic. Fix failures; never skip checks to get green |
| Gate 2 (real data) | Per external-data step | Once, at the end, with authorized data |
| Gate 3 (independent review) | Mandatory before production | Skipped for prototypes. **Required once** before any production release or sharing with real users: one independent pass, fixes in one commit |
| Gates 4–5 (QA, screenshots) | Mandatory before production | Owner smoke test of the main flow at the end. Screenshots only if the owner asks |
| Visuals (§11.1) | Proactive diagrams | Only on request |
| Communication (§11) | Detailed | Terse. Report decisions and results, not process. The §11.2 simple-writing rules still apply in full |

### 13.2 Hard stops (still apply in Fast)

Stop and ask the owner before:
- Anything involving secrets, credentials or personal data. Never commit or print secrets (§7).
- Writing to, or deleting from, any external system or data not named in the brief.
- Adding a paid service, or a dependency the brief did not name or clearly imply. Common libraries for the chosen stack that the brief clearly implies are allowed. Record them in the brief.
- Deploying to production or merging to the production branch, unless the owner delegated it in writing.
- A conflict between the brief and reality that changes scope (wrong schema, missing API, impossible requirement).
- Any use of Astra or another restricted model (§9.3).

### 13.3 Models and subagents

§9 applies unchanged. Fast saves owner time, not model budget: prefer the cheapest adequate model, High effort at most, and delegate only when parallel work clearly pays off.

### 13.4 Graduating to Standard

Switch to Standard when the project gets real users, real data or money flows, or when the owner asks. Before the switch: write the full spec from the brief, record it in the decision log, run gate 3 (independent review) and gates 4–5, and add tests for important logic. The agent should **propose** graduating when it sees these triggers; the owner decides.

### 13.5 Honesty still applies

Fast skips ceremony, not truth. Do not claim a check ran when it did not. Name every skipped gate in the end note (`gate 3: SKIPPED (Fast, prototype)`).
