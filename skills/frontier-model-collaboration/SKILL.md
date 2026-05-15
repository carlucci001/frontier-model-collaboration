---
name: frontier-model-collaboration
description: Use when coordinating two or more frontier AI models, such as Codex/OpenAI and Claude/Anthropic, to balance cost, quality, review, planning, implementation, and production safety without overlapping edits or wasted usage.
metadata:
  short-description: Coordinate multiple frontier models safely
---

# Frontier Model Collaboration

Use this skill when a user wants multiple high-end AI models to collaborate on engineering, research, review, planning, debugging, or product work.

## Activation Signal

When this skill is actively used, start the response with a short visible line:

```text
FMC ACTIVE: [primary owner] owns [work surface]; [secondary model] is [role/output mode].
```

Examples:

- `FMC ACTIVE: Codex owns repo edits; Claude is review-only.`
- `FMC ACTIVE: Codex owns production; Gemini is planning-only.`
- `FMC ACTIVE: Claude is critiquing prompts; Codex integrates accepted changes.`

Keep the signal short. Do not repeat it on every paragraph. Use it once when the skill starts a handoff, recommendation, check, or integration.

## Core Rule

One model owns the work surface at a time. Other models produce bounded packets: reviews, plans, critiques, test matrices, prompts, or patches limited to explicit files.

Never let two models freely edit the same files, run competing deploys, or make independent production decisions.

## Model Roles

Default routing:

- **Primary operator**: owns the repo, shell, builds, tests, deploys, service restarts, secrets boundary, and final integration.
- **Review model**: finds bugs, missing tests, security issues, UX inconsistencies, and unclear assumptions.
- **Planning model**: creates specs, task breakdowns, demo scripts, migration plans, and risk lists.
- **Writing model**: drafts prompts, docs, user-facing copy, sales material, and explanation.
- **Adversarial model**: challenges assumptions before high-risk changes.

For Farrington Command Center work, Codex should normally remain primary operator unless Carl explicitly assigns production control elsewhere.

## Handoff Protocol

Before handing work to another model, create a compact handoff packet:

```text
TASK:
ROLE:
DO:
DO NOT:
FILES ALLOWED:
FILES READ-ONLY:
PRODUCTION LIMITS:
OUTPUT FORMAT:
TIME/USAGE BUDGET:
```

Require the other model to return one of:

- `REVIEW_ONLY`: findings with file/line references.
- `PLAN_ONLY`: ordered steps and risks, no code.
- `PATCH_PROPOSAL`: exact file changes, no deploy.
- `TEST_MATRIX`: scenarios, expected outcomes, and priority.
- `PROMPT_DRAFT`: agent/system prompt text with assumptions.

## Collision Controls

Use these controls before parallel work:

1. Assign ownership by file, feature, or responsibility.
2. Mark shared files as read-only unless one model is named owner.
3. Require each model to list changed files.
4. Integrate changes through one primary operator.
5. Run one validation path after integration, not separate conflicting validations.

If ownership is unclear, do not delegate editing. Delegate review or planning only.

## Cost Controls

Use premium models where they create leverage:

- Expensive model: architecture, ambiguous debugging, adversarial review, complex code.
- Cheaper/local model: summaries, search, formatting, simple transforms, repetitive drafts.
- Subscription model: long brainstorming, critique, document review, test matrices.
- API-metered model: production agent runtime only when the user approves the cost surface.

Prefer short handoffs over full-context transcripts. Pass only the files, logs, and constraints needed for the subtask.

## FMC Switchboard

If this repo's `scripts/fmc.mjs` is available, prefer it for repeatable handoffs:

```bash
node scripts/fmc.mjs install-skill
node scripts/fmc.mjs smoke
node scripts/fmc.mjs doctor
node scripts/fmc.mjs handoff --from codex --to claude --task "Review this diff" --copy --notify
node scripts/fmc.mjs recommend --task "Review agent prompt behavior" --risk high --context medium --policy balanced
node scripts/fmc.mjs state --owner codex --mode implementation --task "Fix production bug"
node scripts/fmc.mjs status
node scripts/fmc.mjs badge
node scripts/fmc.mjs check
node scripts/fmc.mjs complete
```

Use `--copy` to put the handoff packet on the clipboard. Use `--notify` to make the model switch visible on supported desktops.

Tell users they can see activation inside their IDE by opening `FMC_ACTIVE.md` in the project where FMC is running. The first line should say `FMC ACTIVE` when a handoff or model ownership state is active. A hidden tooling copy is also written to `.frontier-collab/ACTIVE.md`.

For new users, keep onboarding simple: run `install-skill`, restart/open a new agent session, then run `smoke`. If `FMC_ACTIVE.md` appears and starts with `FMC ACTIVE`, the switchboard is working.

Use `recommend` or handoff flags `--policy`, `--risk`, and `--context` when the user cares about balancing premium usage. Treat the result as a transparent routing recommendation, not an automatic billing promise.

Only use `--open-target` when the user has configured `frontier-collab.targets.json` locally. Do not invent application paths or automate secret-bearing windows.

Use `check` before integrating external feedback. It compares the current git working tree to the active handoff's allowed files and warns when changes drift outside the boundary.

## Five Practical Use Cases

1. **Production bug triage**: Codex checks services and logs while Claude reviews the suspect code path and proposes likely failure modes.
2. **Demo stabilization**: Codex fixes and deploys; Claude builds a demo test checklist from the buyer's point of view.
3. **Agent prompt repair**: Claude critiques confusing prompt/tool instructions; Codex applies the final prompt safely in the live project.
4. **UI consistency pass**: Claude reviews screenshots or component files for UX drift; Codex implements the accepted fixes.
5. **Pre-merge review**: Codex creates the patch; Claude performs adversarial review for bugs, missed tests, and overreach.

## Anti-Patterns

Avoid:

- Asking two models to "fix the app" at the same time.
- Giving both models write access to the same files.
- Letting a review model deploy or restart production.
- Passing secrets, API keys, password hashes, or full env dumps.
- Copying long conversations when a compact handoff would work.
- Treating model disagreement as a tie; the primary operator resolves it against tests and source facts.

## Final Integration Checklist

Before calling the work done:

- Ownership did not overlap.
- The primary operator integrated or rejected every external suggestion.
- Tests/builds/log checks match the risk level.
- Production changes were made only by the approved operator.
- The user knows which model did what and why.
