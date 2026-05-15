# Frontier Model Collaboration

A lightweight handoff protocol and Agent Skill for safely using multiple frontier AI coding models in the same real project.

This project is for developers who use tools like OpenAI Codex, Claude Code, Gemini CLI, Cursor, OpenCode, OpenClaw, or local LLMs and want the benefits of multi-model work without file collisions, duplicated context, runaway usage, or production chaos.

## The Core Idea

Most multi-agent frameworks answer:

> How do agents talk to each other?

This skill answers:

> How does a human operator safely divide work between multiple powerful AI coding agents?

The default pattern is simple:

- One model is the **primary operator**.
- Other models receive bounded review, planning, critique, prompt, or test tasks.
- Every handoff has an explicit task, role, file boundary, output format, and usage budget.
- One operator integrates the final decision.

## Why This Exists

Developers are already using Codex, Claude, Gemini, and local models together. The problem is that informal multi-model workflows often create:

- overlapping file edits
- stale context
- conflicting advice
- unnecessary premium usage
- unclear production ownership
- agents claiming work they cannot safely perform

This skill turns that into a repeatable operating protocol.

## Install

Copy the skill folder into your agent skills directory:

```bash
skills/frontier-model-collaboration
```

For Codex-style skills, install the folder so the agent can discover `SKILL.md`, then restart or open a new agent session so the skill registry refreshes.

## Use

Ask your coding agent:

```text
Use frontier-model-collaboration to create a Claude review handoff for this bug.
```

Or:

```text
Use frontier-model-collaboration. Codex owns implementation and deploy. Claude should review the diff only and return findings.
```

## FMC Switchboard

This repo also includes a tiny dependency-free CLI called **FMC Switchboard**.

It helps make model handoffs visible:

- generates a clean handoff packet
- shows cost/routing factors for the handoff
- records who owns the work in `~/.frontier-model-collaboration/state.json`
- copies the packet to your clipboard
- optionally shows a desktop notification
- optionally opens a locally configured target, such as Claude, ChatGPT, or your IDE

Run from this repo:

```bash
node scripts/fmc.mjs handoff --from codex --to claude --task "Review this diff for production risk" --files "app/api/auth/login/route.js,lib/auth.js" --copy --notify
```

Or, after installing as a package:

```bash
fmc handoff --from codex --to claude --task "Review this patch" --copy --notify
fmc recommend --task "Review agent prompt transfer behavior" --risk high --context medium --policy balanced
fmc state --owner codex --mode implementation --task "Fix transfer latency"
fmc status
fmc check
fmc complete
```

Cost routing is intentionally transparent rather than magic. FMC shows:

- `policy`: `conserve`, `balanced`, or `max-quality`
- `risk`: `low`, `medium`, or `high`
- `context`: `small`, `medium`, or `large`
- recommended lane: `codex`, `claude`, `gemini`, or `local`
- usage surface: subscription quota, API usage, or local compute
- why that lane was recommended

Example:

```bash
fmc handoff --from codex --to claude --task "Review transfer prompt behavior" --risk high --context medium --policy conserve
```

`fmc check` compares the current git working tree against the active handoff boundary. If the handoff only allowed `app/api/auth/login/route.js` and something else changed, it reports the drift before two models accidentally step on the same work.

To open local targets, copy the example config:

```bash
cp examples/frontier-collab.targets.example.json frontier-collab.targets.json
```

Then run:

```bash
fmc handoff --from codex --to claude --task "Review this patch" --copy --notify --open-target claude-web
```

The target file is intentionally local. This avoids shipping hard-coded app paths or unsafe automation into everyone else's machine.

## Collaboration Ledger

FMC writes local coordination state to:

```text
~/.frontier-model-collaboration/state.json
~/.frontier-model-collaboration/ledger.jsonl
```

The ledger records action type, timestamp, working folder, active owner, and boundary results. It does not log secrets, clipboard contents, model responses, or file contents.

## Good Handoff Packet

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

## Five Practical Workflows

1. **Production bug triage**  
   Primary operator checks services, logs, builds, and deploy safety. Review model examines the suspect code path and returns likely failure modes.

2. **Demo stabilization**  
   Primary operator fixes and verifies. Review model builds a buyer-facing test matrix and catches embarrassing demo gaps.

3. **Agent prompt repair**  
   Review model critiques unclear prompts, tool claims, and transfer behavior. Primary operator applies the accepted prompt changes.

4. **UI consistency review**  
   Review model inspects screenshots or components for UX drift. Primary operator implements only accepted fixes.

5. **Pre-merge adversarial review**  
   Primary operator creates the patch. Review model looks for bugs, security issues, missing tests, and overreach.

## What This Is Not

This is not a replacement for CrewAI, AutoGen, LangGraph, LiteLLM, OpenRouter, or an agent runtime.

It is a small workflow layer for human-supervised engineering teams who want to use multiple frontier models responsibly.

## Repository Layout

```text
package.json
scripts/fmc.mjs
skills/frontier-model-collaboration/SKILL.md
skills/frontier-model-collaboration/agents/openai.yaml
examples/
```

## License

MIT
