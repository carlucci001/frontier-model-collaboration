# Codex To Claude Review Handoff

```text
TASK:
Review the current patch for bugs, missed tests, production risk, and unclear assumptions.

ROLE:
Claude is the review model. Codex remains the primary operator.

DO:
- Review only the files and diff provided.
- Return findings ordered by severity.
- Include file and line references where possible.
- Suggest tests that would catch each issue.

DO NOT:
- Edit files.
- Run deploy commands.
- Restart services.
- Ask for secrets or environment values.
- Redesign unrelated systems.

FILES ALLOWED:
[list exact files or diff]

FILES READ-ONLY:
All files.

PRODUCTION LIMITS:
No production action. Review only.

OUTPUT FORMAT:
REVIEW_ONLY

TIME/USAGE BUDGET:
One concise pass. Prioritize high-impact risks.
```
