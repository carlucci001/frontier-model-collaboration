# Demo Stabilization Test Matrix Handoff

```text
TASK:
Create a buyer-facing test matrix for the demo-critical flows.

ROLE:
Secondary model is a planning/review model. Primary operator owns fixes.

DO:
- List test scenarios by priority.
- Include expected user-visible result.
- Mark anything that could embarrass a live demo.
- Include quick smoke checks and deeper checks separately.

DO NOT:
- Edit code.
- Invent features not present in the app.
- Assume unavailable credentials.
- Tell the user to perform repetitive manual testing if automation is possible.

FILES ALLOWED:
Read-only project context, route names, screenshots, and known bug notes.

FILES READ-ONLY:
All files.

PRODUCTION LIMITS:
No production actions.

OUTPUT FORMAT:
TEST_MATRIX

TIME/USAGE BUDGET:
Focus on demo blockers first.
```
