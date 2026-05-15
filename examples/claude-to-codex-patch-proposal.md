# Claude To Codex Patch Proposal Handoff

```text
TASK:
Convert this review into a scoped patch proposal for Codex to integrate.

ROLE:
Claude proposes changes. Codex owns implementation, tests, build, and deploy.

DO:
- Identify the smallest safe change.
- List exact files that would change.
- Explain why each change is needed.
- Include test expectations.

DO NOT:
- Touch production.
- Claim the patch is deployed.
- Modify files outside the allowed list.
- Include secrets, tokens, or private environment values.

FILES ALLOWED:
[list exact files]

FILES READ-ONLY:
[list context files]

PRODUCTION LIMITS:
No restart, no deploy, no database mutation.

OUTPUT FORMAT:
PATCH_PROPOSAL

TIME/USAGE BUDGET:
Keep it short enough for Codex to verify quickly.
```
